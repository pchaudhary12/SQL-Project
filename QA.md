What are your risk areas? Identify and describe them.

(1) Duplicate Records:
                       Duplicate Records may take up more space, also could be a factor for inaccurate values.

(2) Correct Datatypes :
                       Datatypes should be correct across all tables.

(3) Unique IDs :    
                 some IDs that are cruical to the data shoud be unique

QA Process:

//Describe your QA process and include the SQL queries used to execute it.

I am implimenting a QA process to make sure that we have unique IDs, no duplicates and sensible values across tables

```SQL
-- Starting with all_sessions column 

-- Making sure there is no duplicate fulllvisitorid as there should be distinct id for every user
select 
distinct(fullvisitorid) from all_sessions
```
```SQL
-- Analytics Table
-- unit_price  and units_sold should be in positive numbers and more than zero as to get accurate numbers  revenue 
select * from analytics
where unit_price > 0 and units_sold > 0
```
```SQL
-- Products Table
-- Each product should only have unique id
select distinct(sku) from products
```
```SQL
-- orderedquantity and stocklevel should be more than 0
select * from products
where orderedquantity >= 0 and stocklevel >=0
```
```SQL
-- Total products ordered should be atleast one and stocklevel should be more than zero
select * 
from sales_report
where total_ordered > 0 and stocklevel >= 0
select * from sales_report
```
