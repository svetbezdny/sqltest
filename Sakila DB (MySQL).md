## Get the actors

_Select all records from the actor table._

```sql
select * from actor
```

## Get a list of the actors' names

_Select all the values of the actors' first and last names from the actor table._

```sql
select
    first_name,
    last_name
from actor
```

## Get an ordered list of movies

_Select the movie titles from the table film._  
_Sort the resulting list alphabetically_

```sql
select title from film
order by 1
```

## Get the first 10 movies in alphabetical order

_Select the title, description and year of release of films from the film table._  
_Sort the resulting list by name in alphabetical order and output the first ten lines_

```sql
select
 title,
 description,
 release_year
from film
order by 1
limit 10
```

## Get third page of movie list

_For ease of display, we will divide the list of films into pages of ten entries each._  
_To form the third page of the list, select the title, description and year of release of films from the film table._  
_Sort the resulting list by name in alphabetical order and print ten lines starting from the twenty-first._

```sql
select
 title,
 description,
 release_year
from film
limit 10
offset 20
```

## Get a list of movies sorted by multiple fields

_Select the title, rental rate and length of films from the film table._  
_Sort the resulting list in descending order of rating, films with the same rating sort by ascending length of the film_

```sql
select
 title,
 rental_rate,
 length
from film
order by 2 desc, 3
```

## Get the longest movie

_Find the one longest movie from film table._  
_If more when one movies have same duration get one whith lowest replacement cost_  
_Write the query returns two columns title and release year_

```sql
select
 title,
 release_year
from film
order by length desc, replacement_cost
limit 1
```

## Find long movies

_Find all movies over three hours long._  
_Write a query that returns a result consisting of three columns: the title of the movie, its description and the duration in minutes, sorted by the length of the movie._

```sql
select
 title,
 description,
 length
from film
where length > 180
order by 3
```

## Find long comedies

_Find all comedies over three hours long._
_Write a query that returns a result consisting of three columns: the name of the film, the year of release and the duration in minutes, sorted by the length of the film._

```sql
select
 title,
 release_year,
 length
from film f
join film_category fc on f.film_id = fc.film_id
join category c on fc.category_id = c.category_id
where length > 180
 and c.name = 'Comedy'
order by 3
```

## Find the actors by name

_Get actors who have the first name "Scarlett"._

```sql
select * from actor
where first_name = 'Scarlett'
```

## Find duplicate actor names

_In the previous task, we found that there were two entries in the table about actors named "Scarlett"._  
_Find all names that appear more than once._  
_As a result, you will get a table with two columns, firstname and count._

```sql
select
 first_name,
 count(*) as count
from actor
group by 1
having count(*) > 1
```

## Find the most popular surname among actors

_Find the most popular surname among actors._  
_The result should contain two columns - the last name lastname and the number of actors with this surname count._

```sql
select
 last_name,
 count(*) as count
from actor
group by 1
order by 2 desc
limit 1
```

## Get the languages list

_Get list of values from the name column of the language table._

```sql
select name from language
```

## Get the ordered list of languages

_Get name column from the language table in alphabetical order._

```sql
select name from language
order by 1
```

## Get the sorted list of films with limit

_Get name column from the language table in alphabetical order._

```sql
select
 title
from film
order by length desc
limit 5
```

## Find stuff members by condition

_Find staff members worked in store number 1 and get all theirs data._

```sql
select * from staff
where store_id = 1
```

## Get the sorted list of films with condition

_Find all films longer them 3 hours and get their title, release year and length sorted by length in ascending order._

```sql
select
 title,
 release_year,
 length
from film
where length > 180
order by 3
```

## Find film names by description

_Find all films where description contains the 'Student' word. Output this films titles in alphabetical order._

```sql
select title from film
where description like '%Student%'
order by title
```

## Find clients starting with the letter "A"

_Select last names, first names and email addresses of clients whose last name begins with the letter "A"._  
_Sort results alphabetically by last name._

```sql
select
 last_name,
 first_name,
 email
from customer
where last_name like 'A%'
order by 1
```

## Find clients starting with the letter "A" (2)

_Select last names, first names and email addresses of clients whose first and last names begin with the letter "A"._  
_Sort results alphabetically by last name._

```sql
select
 last_name,
 first_name,
 email
from customer
where last_name like 'A%' and first_name like 'A%'
order by 1
```

## Find customers full names

_Find all store customers with id 2 (storeid = 2)._  
_Select them as a two-column table, where the first column is full name - the customer's first and last name with one space in between, and the second column is the customer's email address._  
_Sort the results by last name in alphabetical order._

```sql
select
 concat_ws(' ', first_name, last_name) as full_name,
 email
from customer
where store_id = 2
order by last_name
```

## Find addresses using sub-query

_Write query that returns address and postal code of all addresses located in London. Don't use JOIN for this task._

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

## Find addresses using JOIN

_Write query that returns address and postal code of all addresses located in London using JOIN._

```sql
select
 address,
 postal_code
from address a
join (
    select * from city c
    where city = 'London'
) c on a.city_id = c.city_id
```

## Find all the actors in the film

_Find all the actors who starred in the movie 'ANGELS LIFE'._  
_Output the resulting table from two columns firstname, lastname sorted by last name in alphabetical order._

```sql
select
 first_name,
 last_name
from actor a
join film_actor fa on a.actor_id = fa.actor_id
join film f on fa.film_id = f.film_id
where f.title = 'ANGELS LIFE'
order by 2
```

## Find all films of an actor

_Find all the films in which HENRY BERRY starred._  
_Output a resulting table of three columns title, releaseyear and rating sorted by rating._

```sql
select
 title,
 release_year,
 rating
from actor a
join film_actor fa on a.actor_id = fa.actor_id
join film f on fa.film_id = f.film_id
where a.first_name = 'HENRY'
 and a.last_name = 'BERRY'
order by 3
```

## Find clients who rented the film

_Find customers who rented the movie 'FRONTIER CABIN'._  
_Write a query that returns a table with columns firstname, lastname and email and rentaldate sorted in ascending order by the dates in the last column_

```sql
select
 first_name,
 last_name,
 email,
 rental_date
from rental r
join inventory i on r.inventory_id = i.inventory_id
join film f on i.film_id = f.film_id
join customer c on r.customer_id = c.customer_id
where title = 'FRONTIER CABIN'
order by 4
```

## Find all films where HENRY BERRY did not participate

_Find all the films in which HENRY BERRY does not starred._  
_Output a resulting table of three columns title, releaseyear and rating sorted by rating._

```sql
select
 title,
 release_year,
 rating
from film
where film_id not in (
    select film_id from actor a
    join film_actor fa on a.actor_id = fa.actor_id
    where first_name = 'HENRY' and last_name = 'BERRY'
)
order by 3
```

## Find the number of films in which the actor took part

_Find the number of films in which HENRY BERRY took part._  
_Name the resulting column filmcount._

```sql
select
    count(*) as film_count
from film_actor fa
join actor a on fa.actor_id = a.actor_id
where concat_ws(' ', first_name, last_name) = 'HENRY BERRY'
```

## Find actors more popular than HENRY BERRY

_Find actors more popular than HENRY BERRY (who have starred in more films)._  
_Display the result in four columns actorid, firstname, lastname and filmcount sorted by increasing popularity._

```sql
with t as (
    select
     a.actor_id,
     a.first_name,
     a.last_name,
     count(*) as film_count
    from film_actor fa
    join actor a on fa.actor_id = a.actor_id
    group by 1, 2, 3
)
select * from t
where film_count > (select film_count from t where concat_ws(' ', first_name, last_name) = 'HENRY BERRY')
order by film_count
```

## Find the distribution of films by category

_Count the number of films belonging to each category._  
_Output the resulting table from two columns category and count in descending order of quantity._

```sql
select
 name as category,
 count(*) as count
from category c
join film_category fc on c.category_id = fc.category_id
join film f on fc.film_id = f.film_id
group by 1
order by 2 desc
```

## Find the average length of a movie

_Find the average length of the movie._
_Display the result in the avg-film-length column._

```sql
select
 avg(length) as avg_film_length
from film
```

## Find minimum, maximum and average film duration

_Find the minimum, maximum and average length of a movie for each category._  
_Display the result in the form of a table with columns: category - name of the movie category, min-length, max-length and аvg-length sorted by category in alphabetical order_

```sql
select
 name as category,
 min(length) as min_length,
 max(length) as max_length,
 avg(length) as avg_length
from category c
join film_category fc on c.category_id = fc.category_id
join film f on fc.film_id = f.film_id
group by 1
order by 1
```

## Find long movie categories

_Find categories with an average movie length of more than two hours._  
_Display the result as a table with columns: category - the name of the movie category and avg-length sorted in descending order of the average movie length_

```sql
select
 name as category,
 avg(length) as avg_length
from category c
join film_category fc on c.category_id = fc.category_id
join film f on fc.film_id = f.film_id
group by 1
having avg(length) > 120
order by 2 desc
```

## Find the minimal and maximal film rental cost

_Find the the minimal and maximal film replacement cost._
_Display the result as two columns table minimal-replacement-cost and maximal-replacement-cost._

```sql
select
    min(replacement_cost) as minimal_replacement_cost,
    max(replacement_cost) as maximal_replacement_cost
from film
```

## Find company stores details

_Find the names of managers and store addresses of the company._  
_Display the result as a table with the columns: store-id, manager and address._  
_The manager column should display the last name and the first letter of the name with a dot after it._  
_For example: Silverman B._  
_Sort the data in ascending order of the store-id field_

```sql
select
 s.store_id,
 concat(last_name, ' ', substr(first_name, 1, 1), '.') as manager,
 a.address
from staff s
join store st on s.store_id = st.store_id
join address a on st.address_id = a.address_id
order by 1
```

## Find film average rental time

_Find the the average film's rental time time._  
_Display the result as one columns table average-rental-time._  
_The rental time calculate as difference between return-date and rental-date in table rental_

```sql
select
 avg(datediff(return_date, rental_date)) as average_rental_time
from rental
```

## Find film average rental time for each customer

_Find the average movie rental time for each customer._
_Display the result as a table with columns customer-id, first-name, last-name and average-rental-time._  
_The rental time is calculated as the difference between return-date and rental-date in the rental table._  
_Round the result up to the nearest whole number. Sort the result in ascending order of the customer-id field._

```sql
select
 r.customer_id,
 first_name,
 last_name,
 ceil(avg(datediff(return_date, rental_date))) as average_rental_time
from rental r
join customer c on r.customer_id = c.customer_id
group by 1, 2, 3
order by 1
```

## Find the average length of a movie by category

_Find the average length of a movie for each category._
_Display the result in two columns category and avg-film-length sorted by category in alphabetical order._

```sql
select
 name as category,
 avg(length) as avg_film_length
from category c
join film_category fc on c.category_id = fc.category_id
join film f on fc.film_id = f.film_id
group by 1
order by 1
```

## Find the average cost of renting a movie by category

_Find the average cost of renting a movie for each category._
_Display the result in two columns category and avg-replacement-cost sorted in descending order of price._

```sql
select
 name as category,
 avg(replacement_cost) as avg_replacement_cost
from category c
join film_category fc on c.category_id = fc.category_id
join film f on fc.film_id = f.film_id
group by 1
order by 2 desc
```

## Find the top 3 most active customers

_Write a query to retrieve the last name, first name, and count of rentals for the top 3 customers with the highest rental counts._  
_The result should be ordered by the rental count in descending order and then by the last name in ascending order._  
_Pay attention on fact several customers can have same number of rentals._  
_The resulting set columns must be next: last-name, first-name, cnt (the customer's rentals count)._

```sql
select
 last_name,
 first_name,
 count(*) as cnt
from customer c
join rental r on c.customer_id = r.customer_id
group by 1, 2
order by 3 desc, 1
limit 3
```

## Find sad actors

_Find actors who have not starred in any comedy. Display the result in two columns first-name, last-name sorted by first and last name._

```sql
select
 first_name,
 last_name
from actor a
where actor_id not in (
    select actor_id from category c
    join film_category fc on c.category_id = fc.category_id
    join film_actor fa on fc.film_id = fa.film_id
    where name = 'Comedy'
)
order by 1, 2
```

## Find the most diverse actors

_Get a list of actors that starred in all films categories and the count of distinct films they have appeared in._  
_The result must contain first-name, last-name and film-count columns and should be ordered by the number of films in descending order and then by the actor's last name._

```sql
select
 first_name,
 last_name,
 count(distinct fa.film_id) as film_count
from actor a
join film_actor fa on a.actor_id = fa.actor_id
join film_category fc on fa.film_id = fc.film_id
group by 1, 2
having count(distinct category_id) = (select count(category_id) from category)
order by 3 desc, 2
```

## Analyze monthly payment

_Write query calculate the total payment amount for each month and display the results in descending order based on the payment month._  
_The query should return only two columns: payment-month - The month of the payment (formatted as "YYYY-MM") and payment-amount - The total payment amount for each month._

```sql
select
 date_format(payment_date, '%Y-%m') as payment_month,
 sum(amount) payment_amount
from payment
group by 1
order by 1 desc
```

## Find the best month

_Write a query to find the month with the highest total payment amount from table payment._  
_The query should return only two columns: payment-month - the month of the payment (formatted as "YYYY-MM") and payment-amount - the total payment amount for the month._

```sql
select
 date_format(payment_date, '%Y-%m') as payment_month,
 sum(amount) payment_amount
from payment
group by 1
order by 2 desc
limit 1
```

## Find the films never been rented

_Find the films never been rented. Write the query returns single column table contains unique film titles in alphabetical order._

```sql
select
 title
from film f
left join inventory i on f.film_id = i.film_id
where inventory_id is null
order by title
```

## Find the most popular film

_Write a query to find the movie with the most rentals in the Sakila database using the inventory, film and rental tables._  
_The query should return only the movie title - name_

```sql
select
 title
from film f
join inventory i on f.film_id = i.film_id
join rental r on i.inventory_id = r.inventory_id
group by 1
order by count(1) desc
limit 1
```

## Analyze rental data for film

_Write the query retrieve information about the rental count for the film "BUCKET BROTHERHOOD" on a monthly basis._  
_The result should contain next columns:_

-   _film - The title of the film._
-   _rental-month - The month and year of the rental date in the format "YYYY-MM"._
-   _rental-count - The count of rentals for each month._  
    _Ordered by the rental-month in ascending order_

```sql
select
 title as film,
 date_format(rental_date, '%Y-%m') as rental_month,
 count(*) as rental_count
from film f
join inventory i on f.film_id = i.film_id
join rental r on i.inventory_id = r.inventory_id
where title = 'BUCKET BROTHERHOOD'
group by 1, 2
order by 2
```

## Find the customers having rented and not returned items

_Find the customers rented and not returned items._  
_Write the query returns table with three columns first-name, last-name and email sorted by name and surname in alphabetical order_

```sql
select
 first_name,
 last_name,
 email
from customer
where customer_id in (
    select customer_id from rental where return_date is null
    )
order by 1, 2
```

## Find average daily film rentals

_Find average daily film rentals. Write the query returns single column daily-average-rents-count_

```sql
select avg(cnt) as daily_average_rents_count from (
 select
  date_format(payment_date, '%Y-%m-%d') as dt,
  count(*) as cnt
 from rental r
 join payment p on r.rental_id = p.rental_id
 group by 1
)t
```

## Calculate daily income for July 2005

_Calculate daily income for July 2005._  
_Write a query that returns a table of two columns, where the first column is date, the second column daily-income is the amount of payment for rental of films released on this day_

```sql
select
 date(payment_date) as date,
 sum(amount) as daily_income
from payment
where date_format(payment_date, '%Y-%m') = '2005-07'
group by 1
order by 1
```

## Find movie distribution by store

_Find the number of movie discs in each category in each of the two stores._  
_Write a query that displays a table with columns: category - the name of the movie category, store-1 and store-2 number of discs with films of this category in the store._  
_Sort the results by category in alphabetical order._

```sql
select
 name as category,
 sum(case when store_id = 1 then 1 else 0 end) as store_1,
 sum(case when store_id = 2 then 1 else 0 end) as store_2
from category c
join film_category fc on c.category_id = fc.category_id
join inventory i on fc.film_id = i.film_id
group by 1
order by 1
```

## Find the distribution of customer activity by day of the week

_Count the number of films rented depending on the day of the week._  
_Write a query that returns a table of two columns dow (day of week) and rentals-count (the number of films rented this day) sorted in descending order of quantity_

```sql
select
 dayofweek(rental_date) as dow,
 count(*) as rentals_count
from rental
group by 1
order by 2 desc
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

## Quarterly earnings analysis

_Write a query to calculate the revenue of each store for each quarter of 2005._  
_The result should consist of the following fields: store-id and I, II, III, IV - amounts of income for each quarter, respectively_

```sql
select
 store_id,
 sum(case when quarter(payment_date) = 1 then amount else 0 end) as "I",
 sum(case when quarter(payment_date) = 2 then amount else 0 end) as "II",
 sum(case when quarter(payment_date) = 3 then amount else 0 end) as "III",
 sum(case when quarter(payment_date) = 4 then amount else 0 end) as "IV"
from payment p
join staff s on p.staff_id = s.staff_id
where year(payment_date) = 2005
group by 1
```

## Find the countries with the most customers

_Find the three countries with the largest number of customers living there._  
_Output the result in two columns: actor and customers-count sorted by the number of clients from largest to smallest._

```sql
select
 country,
 count(*) as customers_count
from country c
join city ci on c.country_id = ci.country_id
join address a on ci.city_id = a.city_id
join customer cu on a.address_id = cu.address_id
group by 1
order by 2 desc
limit 3
```
