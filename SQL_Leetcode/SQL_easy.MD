175. Combine Two Tables

```sql
SELECT p.firstName, p.lastName, a.city, a.state
FROM Person p LEFT JOIN Address a USING(personID)
```
181. Employees Earning More Than Their Managers

```sql
SELECT e.Name AS Employee
FROM Employee e
JOIN Employee m ON e.ManagerId = m.Id
WHERE e.Salary > m.Salary
```

182. Duplicate Emails

```sql
SELECT email from Person
group by email
having count(email) > 1;
```
183. Customers Who Never Order

```sql
SELECT tt.name AS Customers FROM
(
    SELECT cust.name, ord.id 
    FROM Customers cust 
    LEFT JOIN Orders ord
    ON cust.id = ord.customerId
) AS tt
WHERE tt.id IS NULL
```
196. Delete Duplicate Emails

```sql
DELETE FROM Person
WHERE id NOT IN (
  SELECT id
  FROM (
    SELECT MIN(id) AS id
    FROM Person
    GROUP BY email
  ) t
);
```
197. Rising Temperature

```sql
SELECT w1.id
FROM Weather w1
JOIN Weather w2 ON w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
WHERE w1.temperature > w2.temperature
```
511. Game Play Analysis I

```sql
SELECT player_id, min(event_date) AS first_login
FROM Activity
GROUP BY player_id
ORDER BY player_id, event_date
```

577. Employee Bonus

```sql
select e.name, b.bonus from Employee e
left join bonus b on e.empid = b.empid
where ifnull(b.bonus, 0) < 1000;
```
577. Employee Bonus

```sql
select name, bonus
from Employee e
left join Bonus b
on e.empId = b.empId
where bonus is null or bonus < 1000
```
584. Sales Person

```sql
SELECT
    cus.name
FROM 
    Customer AS cus
WHERE
    cus.referee_id != 2 OR
    cus.referee_id IS NULL
```

586. Customer placing the largest number of orders

```sql
Select customer_number from Orders o
group by Customer_number
order by count(customer_number) desc limit 1;
```

595. Big Countries

```sql
select name, population, area from World
having area >= 3000000 OR population >= 25000000;
```

596. Classes More Than 5 Students

```sql
select class 
from courses 
group by class 
having count(distinct student)>=5;
```

607. Sales person

```sql
SELECT
    s.name
FROM
    salesperson s
WHERE
    s.sales_id NOT IN (SELECT
            o.sales_id
        FROM
            orders o
                LEFT JOIN
            company c ON o.com_id = c.com_id
        WHERE
            c.name = 'RED')
;        
```

610. Triangle Judgement

```sql
SELECT x,y,z, 
    (CASE
        WHEN x + y > z AND y + z > x AND x + z > y THEN "Yes"
        ELSE "No"
        END) AS triangle
FROM Triangle;
```

619. Biggest Single Number

```sql
SELECT MAX(num) AS num
FROM  (
    SELECT num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(*) = 1
) AS A;
```

620. Not Boring Movies

```sql
select *
from cinema
where mod(id, 2) = 1 and description != 'boring'
order by rating DESC;
```

197. Rising Temperature

```sql
-- Write your MySQL query statement below
select w1.Id
from Weather w1, Weather w2
where ( DATEDIFF(w1.Date, w2.Date)=1 )
and w1.Temperature > w2.Temperature;
```

596. Classes More Than 5 Students

```sql
-- Write your MySQL query statement below
select class 
from courses 
group by class 
having count(distinct student)>=5;
```

196. Delete Duplicate Emails

```sql
-- Write your MySQL query statement below
delete p1
from Person p1, Person p2
where p1.Email = p2.Email
and p1.Id > p2.Id;
```

176. Second Highest Salary

```sql
-- Write your MySQL query statement below
select
max(Salary) as SecondHighestSalary
from Employee 
where Salary < (select Max(salary) from Employee);
```
570. Managers with at Least 5 Direct Reports

```sql
select Name from Employee
where Id in (
select distinct ManagerId
from Employee
where ManagerId is not null
group by ManagerId
having count(ManagerId) >= 5
)
```

608. Tree Node

```sql
select id,
case
when p_id is null then 'Root'
when id in (select distinct p_id from tree) then 'Inner'
else 'Leaf' end as Type
from tree
order by id
```

612. Shortest Distance in a Plane

```sql
select round(sqrt(power(p1.x-p2.x, 2)+power(p1.y-p2.y, 2)), 2) as shortest
from point_2d p1, point_2d p2
where p1.x <> p2.x or p1.y <> p2.y
order by shortest
limit 1
```

585. Investments in 2016

```sql
select round(sum(TIV_2016), 2) as TIV_2016
from insurance
where
TIV_2015 in (select TIV_2015 from insurance group by TIV_2015 having count(*) > 1)
and (LAT, LON) in (select LAT, LON from insurance group by LAT, LON having count(*) = 1)
```
602. Friend Requests II: Who Has the Most Friends

```sql
select id, count(*) as num
from
    (select requester_id as id
    from request_accepted
    union all
    select accepter_id
    from request_accepted) a
group by id
order by num desc
limit 1
```

580. Count Student Number in Departments

```sql
select dept_name,
ifnull(count(distinct student_id), 0) as student_number
from department d
left join student s
on d.dept_id = s.dept_id
group by d.dept_id
order by student_number desc, dept_name
```
574. Winning Candidate

```sql
select Name
from Candidate c
join
    (
    select CandidateId, count(*) as cnt
    from Vote
    group by CandidateId
    order by cnt desc
    limit 1
    ) a
on c.id = a.CandidateId
```

578. Get Highest Answer Rate Question

```sql
select question_id as survey_log
from
    (
    select question_id, count(answer_id)/count(question_id) as rate
    from survey_log
    group by question_id
    order by rate desc
    limit 1
    ) a
```

614. Second Degree Follower

```sql
select f1.follower, count(distinct f2.follower) as num
from follow f1
join follow f2
on f1.follower = f2.followee
group by f1.follower
order by f1.follower, num desc
```
618. Students Report By Geography

```sql
set @r1 = 0;
set @r2 = 0;
set @r3 = 0;

select
America.name as America, Asia.name as Asia, Europe.name as Europe
from
(select name, @r1:= @r1+1 as id from student where continent = 'America' order by name) America
left join
(select name, @r2:= @r2+1 as id from student where continent = 'Asia' order by name) Asia
on
America.id = Asia.id
left join
(select name, @r3:= @r3+1 as id from student where continent = 'Europe' order by name) Europe
on
America.id = Europe.id
```

571. Find Median Given Frequency of Numbers

```sql
select avg(n1.Number) as median
from Numbers n1
where n1.Frequency >= abs(
    (select sum(n2.Frequency) from Numbers n2 where n1.Number >= n2.Number)
    -
    (select sum(n2.Frequency) from Numbers n2 where n1.Number <= n2.Number)
)
```

178. Rank Scores

```sql
-- Write your MySQL query statement below
select s1.Score, 
(select count(distinct  s2.Score) from Scores s2 where s2.Score >= s1.Score) as Rank
from Scores s1
order by s1.Score desc;
```

569. Median Employee Salary

```sql
select e1.*
from Employee e1, Employee e2
where e1.Company = e2.Company
group by e1.Company, e1.Salary
having sum(case when e1.Salary = e2.Salary then 1 else 0 end)
    >= abs(sum(sign(e1.Salary-e2.Salary)))
order by e1.Company, e1.Salary, e1.Id
```

180. Consecutive Numbers

```sql
-- Write your MySQL query statement below
select distinct l1.Num as ConsecutiveNums
from Logs l1, Logs l2, Logs l3
where l1.ID - 1 = l2.ID
and l1.ID - 2 = l3.ID
and l1.Num = l2.Num
and l1.Num = l3.Num;
```

615. Average Salary: Departments VS Company

```sql
select a.pay_month, a.department_id,
case when a.avg_mon_dept_amount = b.avg_dept_amount then 'same'
when a.avg_mon_dept_amount > b.avg_dept_amount then 'higher'
else 'lower' end as comparison
from
(select Date_Format(pay_date, '%Y-%m') as pay_month, department_id, avg(amount) as avg_mon_dept_amount
from salary s
join employee e
on s.employee_id = e.employee_id
group by pay_month, department_id) a
join
(select Date_Format(pay_date, '%Y-%m') as pay_month, avg(amount) as avg_dept_amount
from salary s
join employee e
on s.employee_id = e.employee_id
group by pay_month) b
on a.pay_month = b.pay_month
```

184. Department Highest Salary

```sql
-- Write your MySQL query statement below
select d.Name as Department, e1.Name as Employee, e1.Salary 
from Department d, Employee e1
where d.Id = e1.DepartmentId
and (DepartmentId, Salary) in
(select e2.DepartmentId, max(e2.Salary) as max_salary from Employee e2 group by e2.DepartmentId) ;
```

579. Find Cumulative Salary of an Employee

```sql
select e1.Id, e1.Month, sum(e2.Salary) as Salary
from Employee e1, Employee e2
where
e1.Id = e2.Id
and e1.Month - e2.Month >= 0
and e1.Month - e2.Month < 3
and (e1.Id, e1.Month) not in (select Id, max(Month) from Employee group by Id)
group by e1.Id, e1.Month
order by e1.Id, e1.Month desc
```

177. Nth Highest Salary

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  set N = N-1;
  RETURN (
      -- Write your MySQL query statement below.
      select distinct Salary from Employee order by Salary desc limit N,1
  );
END
```

601. Human Traffic of Stadium

```sql
-- Write your MySQL query statement below
SELECT t.* FROM stadium t
    LEFT JOIN stadium p1 ON t.id - 1 = p1.id
    LEFT JOIN stadium p2 ON t.id - 2 = p2.id
    LEFT JOIN stadium n1 ON t.id + 1 = n1.id
    LEFT JOIN stadium n2 ON t.id + 2 = n2.id
WHERE (t.people >= 100 AND p1.people >= 100 AND p2.people >= 100)
     OR (t.people >= 100 AND n1.people >= 100 AND n2.people >= 100)
     OR (t.people >= 100 AND n1.people >= 100 AND p1.people >= 100)
ORDER BY id;
```

185. Department Top Three Salaries

```sql
-- Write your MySQL query statement below
select d.Name Department, e1.Name Employee, e1.Salary
from Employee e1 
join Department d
on e1.DepartmentId = d.Id
where 3 > (select count(distinct(e2.Salary)) 
                  from Employee e2 
                  where e2.Salary > e1.Salary 
                  and e1.DepartmentId = e2.DepartmentId);
```

262. Trips and Users

```sql
-- Write your MySQL query statement below

select Request_at as Day, 
round(sum(case when Status like 'cancelled_%' then 1 else 0 end) / count(*),2) as 'Cancellation Rate'
from Trips 
join Users
on Users_Id = Client_Id
where Banned = 'No'
and Request_at between '2013-10-01' and '2013-10-03'
group by Request_at;
```