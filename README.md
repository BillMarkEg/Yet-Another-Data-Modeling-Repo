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
#### incremental load data into dim table in ssis scd (all in Dims) using Microsfor SCD Wizard tool 
<br> SCD (fixed attribute change) : if row changed so data goes to different table. <not common scenario>
<br> SCD (inferred member) : late arriving data so fact can't match it in dim as not yet came                   
<br> 
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
