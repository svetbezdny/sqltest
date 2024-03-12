## Find addresses using sub-query

_Write query that returns address and postal code of all addresses located in London._  
_Don't use JOIN for this task._

```sql
select
 address,
 postal_code
from address a
where exists (
    select * from city c
    where city = 'London'
    and a.city_id = c.city_id
)
```

## Find clients who are not familiar with EMILY DEE films

_Find customers who have never rented a movie starring EMILY DEE._  
_As a result, display a table with columns first-name, last-name - the client's first and last name._  
_Sort the list by customer last name._

```sql
select
 last_name,
 first_name
from customer
where not exists (
    select 1
    from rental r
    join inventory i on r.inventory_id = i.inventory_id
    join film_actor fa on i.film_id = fa.film_id
    join actor a on fa.actor_id = a.actor_id
    where first_name = 'EMILY' and last_name = 'DEE'
    and customer.customer_id = r.customer_id
)
order by 1
```
