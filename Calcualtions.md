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
