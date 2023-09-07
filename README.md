# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

(1) Importing eCommerece Database data to postgresql
(2) Cleaning and transforming data for analytics
(3) Study patterns and finding relations across different tables
(4) Running queries to get business analtyics  

## Process
### Part 1 -  Create Database and Importing data
// Working with postgreSQL
// Creating Tables
// General Syntax for creating Table and Importing Data
```SQL
   create Table table_name
   (
    column_name  data_type,
    column_name2 data_type,
    .
    .,
    primary key (column_name)

   )
   ```
   // General syntax for Copying CSV file
  ```SQL
   copy file_name
   from 'file destination'
   delimeter ',' // mentioning delimiter
   CSV header // mentioning that CSV file have header
   ```
  
### Data Cleaning 

Starting with all_sessions Table
(1) Removing duplicate Rows from table by Creating Temporary Table with distinct rows and then Droping the original table and recreate the table from temporary table

Queries: Below, provide the SQL queries you used to clean your data.
```SQL
// Removing duplicate Rows from table

-- Creating Temporary Table with Distinct value 
create table all_sessions_distinct as 
 select distinct on (fullvisitorid) *
 from all_sessions
 order by fullvisitorid
-- Drop Table all_sessions
 drop table all_sessions
-- recreate table all_sessions and import data from distinct table 
create table all_sessions as 
select *
from all_sessions_distinct
(2) Changing DataTypes

// Change Datatype of transactionid to integer

alter table all_sessions
alter column ecommerceaction_type type int using ecommerceaction_type::integer
// Change Datatype of transactionid to integer

alter table all_sessions
alter column transactionid type int using transactionid::integer
// Change Datatype of transactionrevenue to integer

alter table all_sessions
alter column transactionrevenue type int using transactionrevenue::integer
// Change Datatype of itemquantity to integer

alter table all_sessions
alter column itemquantity type int using itemquantity::integer
// Change Datatype of productrevenue to integer

alter table all_sessions
alter column productrevenue type int using productrevenue::integer
// Change Datatype of productrevenue to integer

alter table all_sessions
alter column productprice type int using productprice::integer
// Change Datatype of visitid to integer

alter table all_sessions
alter column visitid type int using visitid::integer
// Change Datatype of productquatity to integer

alter table all_sessions
alter column productquantity type int using productquantity::integer
// Change DataType of Date column to Date

alter table all_sessions
alter column date type date USING date::date
// Change DataType of Transaction Column to integer

alter table all_sessions
alter column transactions type int using transactions::integer
// Change DataType of totaltransactionrevenue to Float

alter table all_sessions
alter column   totaltransactionrevenue type float  using totaltransactionrevenue::double precision
//Replacing Country names where country value is not availble

select *,
       case when country = '(not set)' then  'N/A'
	   else country
	   end
from all_sessions
// Replacing city names with country where value is not city

update all_sessions
set city = case when city = 'not available in demo dataset' then country
	            when city = '(not set)' then country
		        else city
	       end
	  select *
	  from all_sessions
Droping Empty Columns from All_sessions
alter table all_sessions
drop column searchkeyword
alter table all_sessions
drop column itemrevenue
-- Droping Empty itemquantity column
alter table all_sessions
drop column itemquantity 
##Replacing NUll values

-- Updating currencycode column
update all_sessions
set currencycode = case when currencycode is null then 'N/A'
	                    else currencycode
	               end
-- Updating productvariant column 
update all_sessions
set productvariant = case when productvariant = '(not set)' then 'N/A'
	                    else productvariant
	               end
-- Updating v2categorycolumn 
update all_sessions
set v2productcategory = case when v2productcategory in ('${escCatTitle}','(not set)') then 'N/A'
	                    else v2productcategory
	               end
Cleaning Analytics Table
-- Drop empty column
alter table analytics
drop column userid
-- Change Datatype for unit_price 
alter table analytics
alter column unit_price type float using unit_price::float
-- Change Datatype for revenue 
alter table analytics
alter column revenue type bigint using revenue::bigint
-- Change Datatype for bounces
alter table analytics
alter column bounces type int using bounces::int
-- Change Datatype for pageviews
alter table analytics
alter column pageviews type int using pageviews::int
-- Change Datatype for units_sold
alter table analytics
alter column units_sold type int using units_sold::int
-- Change Datatype for date 
alter table analytics
alter column date type date using date::date
-- Change Datatype for visitid
alter table analytics
alter column visitid type int using visitid::integer
-- Change Datatype for visitnumber 
alter table analytics
alter column visitnumber type int using visitnumber::integer
Cleaning Products Table
-- Change Datatype of sentimentmagnitude
alter table products
alter column sentimentmagnitude type float using sentimentmagnitude::float
-- Change Datatype of senitmentscore
alter table products
alter column sentimentscore type float using sentimentscore::float
-- Change Datatype of restockingleadtime 
alter table products
alter column restockingleadtime type int using restockingleadtime::int
-- Change Datatype of stocklevel
alter table products
alter column stocklevel type int using stocklevel::int
-- Change Datatype of orderedquantity 
alter table products
alter column orderedquantity type int using orderedquantity::int
Changing Datatype and assigning pk for sales_by_sku
--Assigning PK to sales_by_sku
alter table sales_by_sku
add primary key (productsku)
-- Changing datatype of total_ordered
alter table sales_by_sku
alter column total_ordered type int using total_ordered::int
```


## Results



```SQL
// Few insights that we can get from the data

Question 1: Which cities and countries have the highest level of transaction revenues on the site?

SQL Queries:

-- Generating the cities from each country with highest level of transaction revenues
select country, city, total_revenue
from (   -- Creating subquery with totaltransaction revenue by each city within the country
        select country,city,
               max(totaltransactionrevenue) as total_revenue,
	             RANK() OVER (PARTITION BY country ORDER BY max(totaltransactionrevenue) DESC) as rank
        from all_sessions
	      where totaltransactionrevenue is not null
        group by country, city
	 ) as sub
where rank = 1
Answer:

Alt text

Question 2: What is the average number of products ordered from visitors in each city and country?

SQL Queries:

--The average number of products ordered from visitors in each city and country
select country, city ,
        avg(productquantity) as avg_product_ordered
from all_sessions
group by country, city 
having avg(productquantity) <> 0
order by country, city
Answer:

Alt text

Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

SQL Queries:

create temp table overall_cate as
(    -- Product categories of products ordered from visitors in each city and country
   select  country, city, p.name as productname, v2productcategory,
           sum(orderedquantity) as total_products
	  
   from all_sessions s
   join products p on s.productsku = p.sku
   group by country,city,p.name, v2productcategory
   having sum(orderedquantity) <> 0
   order by country, city, total_products desc
)
// Finding pattern in the query

( -- finding top 3 orders with highest volume ordered in each city and country 
        select country, city, v2productcategory, max(total_products) as order,
                'Most Popular' as type 
       from overall_cate
group by country, city, v2productcategory
order by max(total_products) desc
limit 3
)

union all -- Unioning columns from both the results that shows top 3 most and least popular orders placed by volume in each city and country

	
( -- finding top 3 orders with lowest volume ordered in each city and country 
        select  country, city, v2productcategory, 
                min(total_products) as order,
               'Least Popular' as type
        from overall_cate
group by country, city, v2productcategory
order by min(total_products) desc
limit 3
)
Answer:

Product categories of products ordered from visitors in each city and country
Alt text

finding top 3 orders with highest volume ordered in each city and country
Alt text

finding top 3 orders with lowest volume ordered in each city and country
Alt text

Unioning columns from both the results that shows top 3 most and least popular orders placed by volume in each city and country
Alt text

Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?

SQL Queries:

 with sale_by_country as     -- Creating CTE with total products sold in each city from each country with products name
(
        select country, city, p.name as productname,
               sum(orderedquantity) as total_products, 
	       rank() over(partition by country,city order by sum(orderedquantity) desc) as rank -- Ranking the grouped products sold in each country and city 
from all_sessions s
join products p on s.productsku = p.sku
group by country,city,p.name
order by country, city
) 
-- selecting highest selling productnames from each city within each country 
select country, city, productname
from sale_by_country
where rank = 1 
Answer:

Alt text

Question 5: Can we summarize the impact of revenue generated from each city/country?

SQL Queries:

select country, city,
       sum(totaltransactionrevenue) as revenue
from all_sessions 
group by country, city
having sum(totaltransactionrevenue) <> 0
Answer:

Alt text
```


## Challenges 
(1) Most Challenging thing was Data cleaning as it requires a lot of time and effort.
(2) Data was messy and assigning correct DataTypes
 

## Future Goals
(1) Do more data cleaning effectively 
