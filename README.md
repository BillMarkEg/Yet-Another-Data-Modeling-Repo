# Yet-Another-Data-Modeling-Repo
## Enterprise data modeling 
<br> why 3NF is enough -> the more we normalize the more we join so its enough.
<br> 2NF -> attributes should depend on full pk or pk if not composite eg.mnger of hotel table is an entity and don't depend on hotel id. so get hotel id as fk in emp 
<br> 3NF -> direct rely on pk eg. postal code and city rely on streat adreess that depends on pk  so move them all to another entity and get pk of it as fk in hotel
<br> if there is a relationship create it. eg. hotel with floor - hotel with rooms - rooms with floors so its easier in joins (Right)
<br> who goes to who -> one side goes to many side  
<br> many-may -> another table 
<br> 1-1 -> optional goes to mandatory. eg. employee and his car so car id goes to employee as fk can be null but pk can't be.
<br> super and sub -> so if super deleted (pk row) then sub (fk rows) should be deleted too eg. hotel and rooms.
<br> **also check ITI skitch**.
<br> intensive writes -> db 
<br> intensive reads -> dwh
<br> cube -> preaggregations are made. 
<br> Granularity u wanna make dimention on eg : year -> month -> day <..ETC> what level u wanna get kpis on.
<br> Slowly changing dim -> type 0 : don't change like birthdat || type 1 : overwrite change <keep last value >   
<br> type 2 : insert new row with timestamp to know last modification date ||  type 3 keep 2 colums for prev value . 
<br> degenerate dimension is attribute is stored as part of the fact table and not in a separate table. eg. Invoice number in fact table with no dim
<br> Junk Dimension– It is a single table with a combination of different and unrelated attributes to avoid having a large number of foreign keys in the fact table. 
<br> Role play dimension– table that has multiple valid relationships with a fact table so fact table may include foreign keys for both ship date and delivery date. 
<br> An outrigger is a dimension table or entity that is joined to other dimension tables in a starschema eg. productCreationDate in productDim and dateDimention
<br> Measures types in fact table:
<br> Additive : measures that can be aggregated across all of the dimensions in the fact table eg. rev
<br> bill inmon -> enterprise dwh 
<br> ralf kimble -> data-marts for each department (may has 2-5 business processes) 
<br> in Dim we add some indicatorflags eg. isActive , isLast and so on to make quering data easier 
<br> star schema  productDim(productCategoryName as attribute) 
<br> snowflake productDim - productCategoryDim(normalized from productDim (product_id , productCategory)

## Dimentinal modeling Microsfot SQl server 
#### bbar vs set-based sql inserts/updates
<br>  why sur key ? as bus key may change overtime + fastjoins + differnt tables in souce system so we need one key 
<br> Set-based programming is based upon the mathematical concept of a set, and has operators that work on a whole set at a time. Procedural (RBAR) programming is 
<br>  based more  on the traditional computer concepts of files and records. So to increase the salary of all employees in department X by 10%:
<br> Set-based:
<br> UPDATE employees SET salary = salary * 1.10 WHERE department = 'X';
<br> Procedural (extreme example, pseudo-code):
<br> 
<br> OPEN cursor FOR SELECT * FROM employees;
<br> LOOP
<br>    FETCH cursor INTO record;
<br>    EXIT WHEN (no more records to fetch);
<br>    IF record.department = 'X' THEN
<br>       UPDATE employees
<br>       SET    salary = salary * 1.10
<br>       WHERE  employee_id = record.employee_id;
<br>    END IF
<br> END LOOP
<br> CLOSE cursor;
<br> In the procedural version, only one employee row is being updated at a time; in the set-based version, all rows in the "set of employees in department X" are upda <br> ted at once (as far as we are concerned).
<br> Not sure this adds anything to what you will have already read in your links, but I thought I'd have a shot at it!
#### Data Quality Architecture
<br> if ETL failed -> record log in elastic (which step failed + error) then record status of etl (failde/sucess)
<br> should write bd_ingestion_ts to track last touched ETL 
<br> store all natural keys of source systems + add sur key + other attributes to dim and trx to fact
<br>  SCD wizard in ssis ETL tool is item(drag/drop) to make SCD easier 
### Loading Dim tables
#### incremental load data into dim table in ssis scd (all in Dims) using Microsfor SCD Wizard tool 
<br> SCD (fixed attribute change) : if row changed so data goes to different table. <not common scenario> so main table didnt change
<br> SCD (inferred member) : late arriving data so fact can't match it in dim as not yet came                   
<br> SCD (changing attribute ) :SCD type 1  (may there is historical data so if apply to history option is active will update all hist rows to last version)
<br> SCD (historical attribute ) :SCD type 2 (need to show who is last row using A: start n end date B:status flag isACtive) so last date will be null means active 
#### Late arriving Dim == infered dim
when natural key in fact finds no map in dim so there is many options (3rd one in module 5 is best)
<br> create incremental sur key and save natural key in dim and in temp table then if come late will he updated else its as is in dim.
<br> when we select from fact and no mapping key in dim so it will be created with 2 options
<br> 1: is to create it with all null columns 
<br> 2: to add one more column to say if infered/not in dimention tab.
<br> when this infered come it will update that row BTW we can create this all in SCD wizard : check demo in module 3
<br> wizard didnt detect deletion in source system.
#### ALTERNATIVE TO WIZARD
<br> wizard dont support type SCD type 2  / don't detect deletion in source / very slow bbar <can be more faster if choosed fast load option : still bad>
<br> so option2 1-load data to stage table 2-T-sql to laod data (merge) --> this option is fast as it use bulk (set) load 
<br> merge : target -> target table
<br> source -> source table
<br> when matched -> table in both source and target
<br> when not matched by target -> row in source but not in target
<br> when not matched by source -> row in target but not in source 
<br> check example in module 03 demo merge setul .txt <truncate stage then load data into it then merge <update if exist,insert if not>> TYPE 1 SCD exmple
<br> **NOTE SCD is not on table level but column level so name-> type1 if change will overwrite row / status may be type 2 so if change we add new row **
<br> we need separate merge querey to handle each type of SCD
#### temporal table as Dimentions talbles : keep history of deleted or updated data in original table and can query table back n time.
<br> TYPE 1 SCD : will store the overwritten data to historical table , also can help in type 2 by moving cold data to historical version of it.
<br> we can use it also to join with fact table using valide from & valide to : to get all historical rows from histocal table of it. 
<br> cons -> don't support late coming data. 
<br> cons -> it keep whole record not custom columns so if we make some col as tyoe 1 and other as type 2 the historical tab will grow so fast.
<br> cons -> keep timestamp of db so if validity is based on trx time this will not be tracked in case of archiving to historical table 
#### why we need stage table in T-sql option
<br> as in spark we do transformations in rdd/dataframe here its a database so need a table to make transformations in before loading data into new table.
### Loading fact tables
#### loading fact notes
<br> fact fks should'nt be nulls or add default value then 
<br> better to add bd_ingestion_ts for better troubleshoting
<br> last ETL identifier flag.
#### grouping/value band 
<br> used for quering specific group of customer so we join with dim using upper/lower bound join eg. where fact.price > dim.lowe and fact.price < dim.upper 
<br> to optimize --> make a key for each group and join with it instead of using upper/lower bound and add them as features instead.
#### transformations on fact
<br> non-blocking -> like wide trans on spark eg.lookup tasks , calc age from bd and so on that dont need all sources to calc dest.
<br> semi-blovking -> don't need all sources like non-blocking but needs buffer in dist. to write eg.merge,merge join,pivot and union all 
<br> blocking -> narrow in spark eg. aggregations and order .. etc
#### reKeying 
<br> process where we load all Dims in ETL process to map natureal key with sur key  NOting that :: 
<br> type 1 SCD : just there is one row of sur with nat
<br> type 2 : need to select secific values with end time is null to get current active or define specific ts to get time period wanted.
<br> Verbose <in Python too>  used for regex extraction and logging and it has lib in ssis too.
#### ETL time bottleneck catching
<br> 1-check distination : replace writing with count action (called multicast)
<br> 2-check transformations : just dont make any trnasformations
<br> if there is still issue so its the source itself.
<br> u may need to filter type 2 SCD dim before joining
<br> cash all lkps(DIMs)
<br> drop index before insert and reADD after insertion is Done.
<br> increate buffer that ssis uses for transofrmations and loading data -> audto adjust buffer like dynamic allocation in spark
<br> reduce num of rows being processed -> incremental load only changes. (come next module.)
<br> distibute ur data --> mod(10) and each taks filter on specifc num 1-9
<br> why can't do full cash in ssis -> may be u'r adding/removing rows in the same pipeline.

### Exploring incremental ETL

### defalt architecture to build
<br> load data into stage table that truncate at start before loading it (except in option3 next) 
<br> if runType pata is hist-> load full data option || if inc -> so we just inc changes
<br> if hist-> just select * from source to stage  then merge to fact table & need to drop index then reBuild after load
<br> if inc -> so differ from option to another check each one's quereis and procedure 
<br> load data into fact table
<br> update Etl ExtractionKeys lkp that track if Etl succeeded/Not.  
### option 1 : using trigger

<br> make trigger that before any dml in source table take it and add to trigger table that track all changes and its type with change id 
<br> then there is another table that stores last etl and last change id processed 
<br> when there is new change it check last id processed and take all new changes in trigger table then load it to fact table 
<br> check quereies of procedure in module data.
  
### option 2 : using timestamp

<br> instead of using change_id we use ts to know new changed data and keep track of it 
<br> when u update anythin in source must update last mod date column too in both source and fact
<br> this approach can't track deleted rows so need extrac way to handle it.
  
### option 3 : using full diff & hashing  

<br> a stage table for tracking all source data + hashed column for all row. + col that track if source need no change = 0 , need change = 1 , need delete = 2 
<br> note stage table here is not truncated each time as before options <1,2> it stores data from last ETL
<br> another view on source table + hashed column for all row
<br> then we join both together to get update col that trach changes <in stage table> if row not fount in view set to 2 , if changeed set to 1 , nochange set to 0
<br> then we take data need changes and merge with fact table then delete change col =2 and update 1 to 0 again for comming ETLs

#### note  : why do we need stage table each time we process data
<br> to avoid RBAR load and bulk load data at once.
#### note  : where processing of transformation happens
<br> in buffer memory and cors we set in configrations of ssis
#### CDC vs CT <options 4 & 5 build in by microsfot sql server db>
<br> CDC is Async so it dont add overload on source system dml || it also record all changee before and after update with all columun before and after 
<br> CT is Sync but lightweight || keep only new data. || tell only pk that has been changed not column itself.
<br> CDC can't track new column automatically but CT can DO
<br> CT can't handle more than 15M change per day but CDC is stable.
#### big dive into CDC 
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
