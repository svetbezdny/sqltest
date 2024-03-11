## Find average disk idle time

_Find the average time (in days) between the date the drive is returned and the next time it is rented_  
_Write a query that displays one value in the column avg-days-between-rentals_

```sql
select
 avg(datediff(ld, return_date)) as avg_days_between_rentals
from (
    select
     inventory_id,
     return_date,
     lead(rental_date) over (partition by inventory_id order by return_date) as ld
    from rental
)t
```

## Find the relative shares of each movie category

_Find the relative share of each category of films in the total number of films issued and the total amount of payments._  
_Write a query that displays a table with columns category, relative-rental-amount and relative-rentals-count._  
_Sort the result by descending field relative-rental-amount_

```sql
select
 name as category,
 sum(amount) / sum(sum(amount)) over () as relative_rental_amount,
 count(name) / sum(count(name)) over () as relative_rentals_count
from category c
join film_category fc on c.category_id = fc.category_id
join inventory i on fc.film_id = i.film_id
join rental r on i.inventory_id = r.inventory_id
join payment p on r.rental_id = p.rental_id
group by 1
order by 2 desc
```

## Find highly paid employees (2)

_Find employees with the maximum salary in his department._  
_Display a table with columns DEPARTMENT - department name, EMP-NO, FIRST-NAME, LAST-NAME - employee data, SALARY - his salary._  
_Sort the data in descending order of salary._

```sql
select
 DEPARTMENT,
 EMP_NO,
 FIRST_NAME,
 LAST_NAME,
 SALARY
from (
    select
     DEPARTMENT,
     EMP_NO,
     FIRST_NAME,
     LAST_NAME,
     SALARY,
     row_number() over(partition by DEPARTMENT order by SALARY desc) as rn
    from EMPLOYEE e
    join DEPARTMENT d on e.DEPT_NO = d.DEPT_NO
    order by 4 desc
) t
where rn = 1
order by 5 desc
```

## Create a salary rating

_Rank employees depending on their salary so that the employee with the maximum salary is in first place._  
_Output the resulting table from three columns FULL-NAME, SALARY and RANK._  
_Sort the results in descending order of rating_

```sql
select
 FULL_NAME,
 SALARY,
 rank() over (order by salary desc) as RANK
from EMPLOYEE
```

## Find movie popularity rating

_Write a request to construct a rating of the popularity of films (by the number of copies rented) in 2005._  
_Get the top three places in the rating (if several films have the same rating, print them all)._  
_Present the result as table of two columns film-rank and film-title_

```sql
select
 film_rank,
 film_title
from (
 select
  title as film_title,
  dense_rank() over(order by count(*) desc) as film_rank
 from film f
 join inventory i on f.film_id = i.film_id
 join rental r on i.inventory_id = r.inventory_id
 where year(rental_date) = 2005
 group by 1
)t
where film_rank <= 3
```
