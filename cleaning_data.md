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