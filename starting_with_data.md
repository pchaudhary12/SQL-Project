Question 1: Find minimum revenue for each fullvisitorid

SQL Queries:
```SQL
-- finding minimum revenue for each fullvisitorid
select fullvisitorid, min(revenue) lowrevenue
from (  -- finding revenue for each fullvisitorid 
        select fullvisitorid,  sum(revenue) as revenue
        from analytics 
        where  revenue is not null
        group by fullvisitorid
        order by revenue desc
	)	sub								
group by fullvisitorid
```
Answer: 

![Alt text](<Screenshot 2023-09-06 031819.png>)

Question 2: List countries and their revenue by each year

SQL Queries:
```SQl

            select country, 
                   extract(year from date) as year, 
                   sum(totaltransactionrevenue) as totalrev
            from all_sessions
            group by country, extract(year from date)
            order by year, country
```
Answer:

![Alt text](<Screenshot 2023-09-06 031919.png>)

Question 3: Show highest selling product with highest and lowest sentimentscore (show totalproductordered, only top 5)

SQL Queries:
```SQl 
-- Top 5 highest selling products with highest sentimentscore
(select name as productname, 
        max(sentimentscore) as sentimentscore, 
        sum(total_ordered) total_product_ordered,
        'Highest' as sentiment_score
from sales_report
group by name
order by  max(sentimentscore)  desc, sum(total_ordered) desc
limit 5)

union all -- Showing all the results together in one table

-- Top 5 highest selling products with lowest sentimentscore
(select name as productname,
         min(sentimentscore) as sentimentscore, 
         sum(total_ordered)  total_product_ordered,
         'Lowest' as sentiment_score
from sales_report
group by name
order by min(sentimentscore),sum(total_ordered) desc
limit 5)
```

Answer:

![Alt text](<Screenshot 2023-09-06 032033.png>)

