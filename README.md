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
