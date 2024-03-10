## Generate dates table

_Using recursive CTE write the query returning single columns table date contains all dates from 2005-07-01 to 2005-07-31_

```sql
with recursive cte as (
 select str_to_date('2005-07-01', '%Y-%m-%d') as date
  union all
 select date_add(date, interval 1 day) as date
 from cte where date < '2005-07-31'
)
select * from cte
```

## Calculate the number of days off in a month

_Using the solution to the previous problem, write a query to count the number of weekends (Saturday and Sunday) in July 2005._  
_The result should contain a single value in the weekend-days column_

```sql
with recursive cte as (
 select str_to_date('2005-07-01', '%Y-%m-%d') as date
  union all
 select date_add(date, interval 1 day) as date
 from cte where date < '2005-07-31'
)
select count(*) as weekend_days from cte
where weekday(date) in (5, 6)
```

## Calculate factorial

_Write a query that returns a table of factorial values for integers from 0 to 10._  
_The table must contain two columns n - a number from 0 to 10 and f the factorial value of this number_

```sql
with recursive cte as (
 select 0 as n, 1 as f
  union all
 select n + 1, f * (n + 1)
 from cte where n < 10
)
select * from cte
```
