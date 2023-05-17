185. Department Top Three Salaries

```sql
SELECT D.Name as Department, E.Name as Employee, E.Salary 
FROM Department D, Employee E, Employee E2  
WHERE D.ID = E.DepartmentId and E.DepartmentId = E2.DepartmentId and 
E.Salary <= E2.Salary
group by D.ID,E.Name having count(distinct E2.Salary) <= 3
order by D.Name, E.Salary desc;
```

262. Trips and Users

```sql
with a as
(select * from Trips
where Client_Id not in (select Users_Id from Users where Banned = 'Yes')
and Driver_Id not in (select Users_Id from Users where Banned = 'Yes'))
select Request_at as Day,
round(avg(case when Status = 'completed' then 0 else 1 end),2) as 'Cancellation Rate'
from a
group by Day
having Day between '2013-10-01' and '2013-10-03'
```

601. Human Traffic of Stadium

```sql
select * from Stadium where id in ( 
select * from (
select s1.id from Stadium s1, stadium s2, stadium s3 
where s2.id = s1.id + 1  and  s3.id = s1.id + 2  and s3.id = s2.id + 1
and s1.people >= 100 and coalesce(s2.people,0) >= 100 and coalesce(s3.people, 0) >= 100

union

select s1.id from stadium s1, stadium s2, stadium s3 
where s2.id = s1.id + 1  and  s3.id = s1.id - 1 and s3.id = s2.id - 2
and s1.people >= 100 and coalesce(s2.people, 0) >= 100 and coalesce(s3.people, 0) >= 100

union

select s1.id from stadium s1, stadium s2, stadium s3 
where s2.id = s1.id -1  and  s3.id = s1.id -2 and s3.id = s2.id - 1
and s1.people >= 100 and coalesce(s2.people, 0) >= 100 and coalesce(s3.people, 0) >= 100) s)
```