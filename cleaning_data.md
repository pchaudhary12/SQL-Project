What issues will you address by cleaning the data?
### Starting with all_sessions Table
/ Removing duplicate Rows from table by Creating Temporary Table with distinct rows and then Droping the original table and recreate the table from temporary table




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
