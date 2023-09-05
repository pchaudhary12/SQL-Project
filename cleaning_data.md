What issues will you address by cleaning the data?
### Starting with all_sessions Table
(1) Removing duplicate Rows from table by Creating Temporary Table with distinct rows and then Droping the original table and recreate the table from temporary table




Queries:
Below, provide the SQL queries you used to clean your data.

// Removing duplicate Rows from table
```SQL
-- Creating Temporary Table with Distinct value 
create table all_sessions_distinct as 
 select distinct on (fullvisitorid) *
 from all_sessions
 order by fullvisitorid
```

```SQL
-- Drop Table all_sessions
 drop table all_sessions
```
``` SQL
-- recreate table all_sessions and import data from distinct table 
create table all_sessions as 
select *
from all_sessions_distinct
```
(2) Changing DataTypes 

// Change Datatype of transactionid to integer
```SQL
alter table all_sessions
alter column ecommerceaction_type type int using ecommerceaction_type::integer
```

// Change Datatype of transactionid to integer

```SQL
alter table all_sessions
alter column transactionid type int using transactionid::integer
```

// Change Datatype of transactionrevenue to integer
```SQL
alter table all_sessions
alter column transactionrevenue type int using transactionrevenue::integer
```

// Change Datatype of itemquantity to integer
```SQL
alter table all_sessions
alter column itemquantity type int using itemquantity::integer
```

// Change Datatype of productrevenue to integer
```SQL
alter table all_sessions
alter column productrevenue type int using productrevenue::integer
```

// Change Datatype of productrevenue to integer
```SQL
alter table all_sessions
alter column productprice type int using productprice::integer
```

// Change Datatype of visitid to integer
```SQL
alter table all_sessions
alter column visitid type int using visitid::integer
```

// Change Datatype of productquatity to integer
```SQL
alter table all_sessions
alter column productquantity type int using productquantity::integer
```

// Change DataType of Date column to Date 
```SQL
alter table all_sessions
alter column date type date USING date::date
```

// Change DataType of Transaction Column to integer
```SQL
alter table all_sessions
alter column transactions type int using transactions::integer
```

// Change DataType of totaltransactionrevenue to Float
```SQL
alter table all_sessions
alter column   totaltransactionrevenue type float  using totaltransactionrevenue::double precision
``` 
//Replacing Country names where country value is not availble
```SQL
select *,
       case when country = '(not set)' then  'N/A'
	   else country
	   end
from all_sessions
```

// Replacing city names with country where value is not city 
```SQL
update all_sessions
set city = case when city = 'not available in demo dataset' then country
	            when city = '(not set)' then country
		        else city
	       end
	  select *
	  from all_sessions
```

## Droping Empty Columns from All_sessions
```SQL
alter table all_sessions
drop column searchkeyword
```
```SQL
alter table all_sessions
drop column itemrevenue
```
```SQL
-- Droping Empty itemquantity column
alter table all_sessions
drop column itemquantity 
```

##Replacing NUll values 

```SQL
-- Updating currencycode column
update all_sessions
set currencycode = case when currencycode is null then 'N/A'
	                    else currencycode
	               end
```

```SQL
-- Updating productvariant column 
update all_sessions
set productvariant = case when productvariant = '(not set)' then 'N/A'
	                    else productvariant
	               end
```
```SQL
-- Updating v2categorycolumn 
update all_sessions
set v2productcategory = case when v2productcategory in ('${escCatTitle}','(not set)') then 'N/A'
	                    else v2productcategory
	               end
```

### Cleaning Analytics Table
```SQL
-- Drop empty column
alter table analytics
drop column userid
```

```SQL
-- Change Datatype for unit_price 
alter table analytics
alter column unit_price type float using unit_price::float
```

```SQL
-- Change Datatype for revenue 
alter table analytics
alter column revenue type bigint using revenue::bigint
```

```SQL
-- Change Datatype for bounces
alter table analytics
alter column bounces type int using bounces::int
```
```SQL
-- Change Datatype for pageviews
alter table analytics
alter column pageviews type int using pageviews::int
```
```SQL
-- Change Datatype for units_sold
alter table analytics
alter column units_sold type int using units_sold::int
```
```SQL
-- Change Datatype for date 
alter table analytics
alter column date type date using date::date
```
```SQL
-- Change Datatype for visitid
alter table analytics
alter column visitid type int using visitid::integer
```
```SQL
-- Change Datatype for visitnumber 
alter table analytics
alter column visitnumber type int using visitnumber::integer
```

### Cleaning Products Table

```SQL
-- Change Datatype of sentimentmagnitude
alter table products
alter column sentimentmagnitude type float using sentimentmagnitude::float
```
```SQL
-- Change Datatype of senitmentscore
alter table products
alter column sentimentscore type float using sentimentscore::float
```
```SQL
-- Change Datatype of restockingleadtime 
alter table products
alter column restockingleadtime type int using restockingleadtime::int
```
```SQL
-- Change Datatype of stocklevel
alter table products
alter column stocklevel type int using stocklevel::int
```
```SQL
-- Change Datatype of orderedquantity 
alter table products
alter column orderedquantity type int using orderedquantity::int
```