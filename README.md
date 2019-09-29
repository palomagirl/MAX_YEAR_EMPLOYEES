# MAX_YEAR_EMPLOYEES
This is a step by step approach to get max year for employees in company

/*
CoderPad provides a basic SQL sandbox with the following schema.
You can also use commands like `show tables` and `desc employees`

employees                             projects
+---------------+---------+           +---------------+---------+
| id            | int     |<----+  +->| id            | int     |
| first_name    | varchar |     |  |  | title         | varchar |
| last_name     | varchar |     |  |  | start_date    | date    |
| salary        | int     |     |  |  | end_date      | date    |
| department_id | int     |--+  |  |  | budget        | int     |
+---------------+---------+  |  |  |  +---------------+---------+
                             |  |  |
departments                  |  |  |  employees_projects
+---------------+---------+  |  |  |  +---------------+---------+
| id            | int     |<-+  |  +--| project_id    | int     |
| name          | varchar |     +-----| employee_id   | int     |
+---------------+---------+           +---------------+---------+
*/

/* Employees with department and salary details*/
--------------
SELECT e.first_name, e.last_name, e.salary,
  d.name as department_name
FROM employees   AS e
JOIN departments AS d ON e.department_id = d.id
--------------

+------------+-----------+--------+-----------------+
| first_name | last_name | salary | department_name |
+------------+-----------+--------+-----------------+
| John       | Smith     |  20000 | Reporting       |
| Cailin     | Ninson    |  30000 | Engineering     |
| Mike       | Peterson  |  20000 | Engineering     |
| Ian        | Peterson  |  80000 | Engineering     |
| John       | Mills     |  50000 | Marketing       |
| Ava        | Muffinson |  10000 | Silly Walks     |
+------------+-----------+--------+-----------------+
6 rows in set (0.00 sec)

/*Employees with project title, project start and end date
We are simulating Start and end date for hire date and termination date*/
--------------
select e.id,e.last_name,title,start_Date,end_Date
from employees e, projects p, employees_projects ep
where e.id= ep.employee_id and ep.project_id=p.id
--------------

+----+-----------+--------------------------+------------+------------+
| id | last_name | title                    | start_Date | end_Date   |
+----+-----------+--------------------------+------------+------------+
|  3 | Ninson    | Build a cool site        | 2011-10-28 | 2012-01-26 |
|  4 | Peterson  | Build a cool site        | 2011-10-28 | 2012-01-26 |
|  5 | Peterson  | Build a cool site        | 2011-10-28 | 2012-01-26 |
|  1 | Smith     | Update TPS Reports       | 2011-07-20 | 2011-10-28 |
|  2 | Muffinson | Design 3 New Silly Walks | 2009-05-11 | 2009-08-19 |
+----+-----------+--------------------------+------------+------------+
5 rows in set (0.01 sec)

--------------
select e.id as employee,
    case 
    when (year(start_Date)<=2000 and year(end_date)>=2000) then '2000'
    when(year(start_Date)<=2001 and year(end_date)>=2001) then '2001'
    when(year(start_Date)<=2009 and year(end_date)>=2009) then '2009'
    when(year(start_Date)<=2010 and year(end_date)>=2010) then '2010'
    when(year(start_Date)<=2011 and year(end_date)>=2011) then '2011'
    else null end as incompany
from employees e, projects p, employees_projects ep
where e.id= ep.employee_id and ep.project_id=p.id
--------------

+----------+-----------+
| employee | incompany |
+----------+-----------+
|        1 | 2011      |
|        3 | 2011      |
|        4 | 2011      |
|        5 | 2011      |
|        2 | 2009      |
+----------+-----------+
5 rows in set (0.00 sec)

--------------
select 
case 
 when incompany = '2000' then '2000' 
 when incompany = '2001' then '2001' 
 when incompany = '2009' then '2009' 
 when incompany = '2010' then '2010' 
 when incompany = '2011' then '2011' end as year_company_employees
from(
  select e.id,
    case 
    when (year(start_Date)<=2000 and year(end_date)>=2000) then '2000'
    when(year(start_Date)<=2001 and year(end_date)>=2001) then '2001'
    when(year(start_Date)<=2009 and year(end_date)>=2009) then '2009'
    when(year(start_Date)<=2010 and year(end_date)>=2010) then '2010'
    when(year(start_Date)<=2011 and year(end_date)>=2011) then '2011'
    else null end as incompany
  from employees e, projects p, employees_projects ep
  where e.id= ep.employee_id and ep.project_id=p.id)a
--------------

+------------------------+
| year_company_employees |
+------------------------+
| 2011                   |
| 2011                   |
| 2011                   |
| 2011                   |
| 2009                   |
+------------------------+
5 rows in set (0.00 sec)

--------------
select 
case 
 when incompany = '2000' then '2000' 
 when incompany = '2001' then '2001' 
 when incompany = '2009' then '2009' 
 when incompany = '2010' then '2010' 
 when incompany = '2011' then '2011' end as year_company_employees, 
count(1) as employees_total
from(
  select e.id,
    case 
    when (year(start_Date)<=2000 and year(end_date)>=2000) then '2000'
    when(year(start_Date)<=2001 and year(end_date)>=2001) then '2001'
    when(year(start_Date)<=2009 and year(end_date)>=2009) then '2009'
    when(year(start_Date)<=2010 and year(end_date)>=2010) then '2010'
    when(year(start_Date)<=2011 and year(end_date)>=2011) then '2011'
    else null end as incompany
  from employees e, projects p, employees_projects ep
  where e.id= ep.employee_id and ep.project_id=p.id)a
group by 1
--------------

+------------------------+-----------------+
| year_company_employees | employees_total |
+------------------------+-----------------+
| 2011                   |               4 |
| 2009                   |               1 |
+------------------------+-----------------+
2 rows in set (0.00 sec)

--------------
select year_company_employees as year_with_most_emp
from(
select 
case 
 when incompany = '2000' then '2000' 
 when incompany = '2001' then '2001' 
 when incompany = '2009' then '2009' 
 when incompany = '2010' then '2010' 
 when incompany = '2011' then '2011' end as year_company_employees, 
count(1) as employees_total
from(
  select e.id,
    case 
    when (year(start_Date)<=2000 and year(end_date)>=2000) then '2000'
    when(year(start_Date)<=2001 and year(end_date)>=2001) then '2001'
    when(year(start_Date)<=2009 and year(end_date)>=2009) then '2009'
    when(year(start_Date)<=2010 and year(end_date)>=2010) then '2010'
    when(year(start_Date)<=2011 and year(end_date)>=2011) then '2011'
    else null end as incompany
  from employees e, projects p, employees_projects ep
  where e.id= ep.employee_id and ep.project_id=p.id)a
group by 1
order by employees_total desc
limit 1)b
--------------

+--------------------+
| year_with_most_emp |
+--------------------+
| 2011               |
+--------------------+
1 row in set (0.00 sec)

