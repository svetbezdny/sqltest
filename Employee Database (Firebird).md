## Find foreign employees

_Write a query for retrieve list of all employees working outside the US._  
_The result must contain all columns from the table EMPLOYEE._

```sql
select * from EMPLOYEE
where JOB_COUNTRY != 'USA'
```

## Find employees by hire date

_Write a query that retrieves a list of all employees hired in 1992._  
_The result should contain the following columns FULL-NAME - the full name of the employee and HIRE-DATE - the hiring date._  
_Sort the results by ascending date of appointment_

```sql
select
 LAST_NAME || ', ' || FIRST_NAME as FULL_NAME,
 HIRE_DATE
from EMPLOYEE
where extract(year from HIRE_DATE) = 1992
order by 2
```

## Find the department

_Find the department managed by Roberto Ferrari._  
_As a result, you will display the number, name, location and telephone number of this unit._

```sql
select
 d.DEPT_NO,
 DEPARTMENT,
 LOCATION,
 PHONE_NO
from DEPARTMENT d
join EMPLOYEE e on MNGR_NO = EMP_NO
where FULL_NAME = 'Ferrari, Roberto'
```

## Find all employees works on the project

_Find all employees working on the "Video Database" project Write a request indicating the employee's number, first name, last name, date of appointment and position code._ _Sort the results by last name in alphabetical order._

```sql
select
 e.EMP_NO,
 FIRST_NAME,
 LAST_NAME,
 HIRE_DATE,
 JOB_CODE
from EMPLOYEE e
join EMPLOYEE_PROJECT ep on e.EMP_NO = ep.EMP_NO
join PROJECT p on ep.PROJ_ID = p.PROJ_ID
where PROJ_NAME = 'Video Database'
order by 3
```

## Find all customers with unshipped orders

_Write a query to find all customers with paid, but unshipped orders._  
_Output the result as a table with two columns CUSTOMER - customer's name and FIRST-ORDER-DATE - date of the first unshipped order_  
_Sort the result table by date in ascendent order_

```sql
select
 CUSTOMER,
 min(ORDER_DATE) as FIRST_ORDER_DATE
from CUSTOMER c
join SALES s on c.CUST_NO = s.CUST_NO
where ORDER_STATUS != 'shipped'
group by CUSTOMER
order by 2
```

## Find the number of employees

_Find the number of employees in each department._  
_Output the name of the department DEPARTMENT and the number of employees in it EMP-COUNT._  
_Sort the results in descending order of number of employees._

```sql
select
 DEPARTMENT,
 count(FULL_NAME) as EMP_COUNT
from DEPARTMENT d
left join EMPLOYEE e on d.DEPT_NO = e.DEPT_NO
group by 1
order by 2 desc
```

## Find highly paid employees

_Write a query that retrieves a list of all employees whose salary is higher than the salary of their department manager._  
_The result should contain the following columns EMP-NO, FIRST-NAME, LAST-NAME - employee data, DEPARTMENT - the name of the department in which he works, SALARY - his salary, MANAGER-SALARY - department manager salary._  
_Sort the result by EMP-NO in default order._

```sql
select
 e.EMP_NO,
 FIRST_NAME,
 LAST_NAME,
 DEPARTMENT,
 SALARY,
 MANAGER_SALARY
from EMPLOYEE e
join DEPARTMENT d on e.DEPT_NO = d.DEPT_NO
join (
 select EMP_NO, SALARY as MANAGER_SALARY from EMPLOYEE
) m on d.MNGR_NO = m.EMP_NO
where SALARY > MANAGER_SALARY
order by 1
```

## Create a salary rating

_Rank employees depending on their salary so that the employee with the maximum salary is in first place._  
_Output the resulting table from three columns FULL-NAME, SALARY and RANK. Sort the results in descending order of rating_

```sql
select
 FULL_NAME,
 SALARY,
 rank() over (order by salary desc) as RANK
from EMPLOYEE
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

## Find valuable employees

_Find employees whose salaries were increased several times within one year._  
_The result should contain the following columns EMP-NO, FIRST-NAME, LAST-NAME - employee data, CHANGES-YEAR - year of salary increase, CHANGES-COUNT - number of increases per year. Sort the result by employee number_

```sql
select
 e.EMP_NO,
 FIRST_NAME,
 LAST_NAME,
 extract(year from CHANGE_DATE) as CHANGES_YEAR,
 count(*) as CHANGES_COUNT
from EMPLOYEE e
join SALARY_HISTORY sh on e.EMP_NO = sh.EMP_NO
group by 1, 2, 3, 4
having count(*) > 1
order by 1
```

## Find the salary ratio

_Write a query that calculates the ratio of the minimum salary in a department to the maximum._  
_Display a table of two columns DEPARTMENT - the name of the department in which he works and SALARY-DIFF-RATIO sorted in descending order of the coefficient_

```sql
select
 DEPARTMENT,
 cast(mins as float) / maxs as SALARY_DIFF_RATIO
from (
    select
     DEPARTMENT,
     min(SALARY) as mins,
     max(SALARY) as maxs
    from EMPLOYEE e
    join DEPARTMENT d on e.DEPT_NO = d.DEPT_NO
    group by 1
)t
order by 2 desc
```
