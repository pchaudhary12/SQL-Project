Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```SQL
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
```     


Answer:




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```SQL 
--The average number of products ordered from visitors in each city and country
select country, city ,
        avg(productquantity) as avg_product_ordered
from all_sessions
group by country, city 
order by country, city
```


Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







