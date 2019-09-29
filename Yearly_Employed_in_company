

select incompany as year_of_max_emp
from(
select
  case 
    when (year(start_Date)<=2000 and year(end_date)>=2000) then '2000'
    when(year(start_Date)<=2001 and year(end_date)>=2001) then '2001'
    when(year(start_Date)<=2009 and year(end_date)>=2009) then '2009'
    when(year(start_Date)<=2010 and year(end_date)>=2010) then '2010'
    when(year(start_Date)<=2011 and year(end_date)>=2011) then '2011'
    else null end as incompany,count(1) as employees_total
from employees e, projects p, employees_projects ep
where e.id= ep.employee_id and ep.project_id=p.id
group by 1
order by employees_total desc
limit 1)a
