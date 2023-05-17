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

627. Swap Salary

```sql
UPDATE salary SET sex = CASE WHEN sex = 'm' THEN 'f' ELSE 'm' END;
```

1050. Actors and Directors Who Cooperated At Least Three Times

```sql
SELECT actor_id, director_id
FROM ActorDirector
GROUP BY actor_id, director_id
HAVING COUNT(timestamp) >= 3;
```

1068. Product Sales Analysis I

```sql
-- Write your MySQL query statement below
delete p1
from Person p1, Person p2
where p1.Email = p2.Email
and p1.Id > p2.Id;
```

1075. Project Employees I

```sql
SELECT P.project_id, ROUND((SUM(E.experience_years)/COUNT(E.experience_years)), 2) as average_years
FROM Employee E
INNER JOIN Project P USING(employee_id)
GROUP BY P.project_id
```

1084. Sales Analysis III

```sql
SELECT p.product_id, p.product_name
FROM Product p
WHERE product_id in (
    SELECT product_id
    FROM Sales
    GROUP BY 1
    HAVING MAX(sale_date) <= CAST('2019-03-31' AS DATE)
    AND MIN(sale_date) >= CAST('2019-01-01' AS DATE)
);
```

1141. User Activity for the Past 30 Days I

```sql
SELECT activity_date as day, COUNT(DISTINCT user_id) as active_users
FROM Activity
WHERE datediff('2019-07-27',activity_date)<30 
AND activity_date <= '2019-07-27' 
GROUP BY activity_date;
```

1148. Article Views I

```sql
select distinct(author_id) as id
from Views
where author_id = viewer_id
order by author_id
```

1179. Reformat Department Table

```sql
# Write your MySQL query statement below
select id, 
	sum(case when month = 'jan' then revenue else null end) as Jan_Revenue,
	sum(case when month = 'feb' then revenue else null end) as Feb_Revenue,
	sum(case when month = 'mar' then revenue else null end) as Mar_Revenue,
	sum(case when month = 'apr' then revenue else null end) as Apr_Revenue,
	sum(case when month = 'may' then revenue else null end) as May_Revenue,
	sum(case when month = 'jun' then revenue else null end) as Jun_Revenue,
	sum(case when month = 'jul' then revenue else null end) as Jul_Revenue,
	sum(case when month = 'aug' then revenue else null end) as Aug_Revenue,
	sum(case when month = 'sep' then revenue else null end) as Sep_Revenue,
	sum(case when month = 'oct' then revenue else null end) as Oct_Revenue,
	sum(case when month = 'nov' then revenue else null end) as Nov_Revenue,
	sum(case when month = 'dec' then revenue else null end) as Dec_Revenue
from department
group by id
order by id
```
1211. Queries Quality and Percentage

```sql
SELECT query_name, 
    ROUND(AVG(rating/position),2) as quality, 
    ROUND(100.0*sum(if(rating<3, 1,0))/count(1),2) as poor_query_percentage
FROM Queries 
GROUP BY query_name;
```

1251. Average Selling Price
    
```sql
where u.purchase_date between p.start_date and p.end_date
```

1280. Students and Examinations

```sql
WITH tot as(
    SELECT *
    FROM Students, Subjects
)
SELECT
      tot.student_id, tot.student_name, tot.subject_name,
      COUNT(ex.student_id) as attended_exams
FROM
    tot LEFT JOIN Examinations ex 
    ON tot.student_id = ex.student_id AND tot.subject_name = ex.subject_name
GROUP BY
        tot.student_id,
        tot.student_name,
        tot.subject_name
ORDER BY student_id, subject_name
```

1327. List the Products Ordered in a Period

```sql
SELECT a.product_name, SUM(b.unit) as unit
FROM Products as a
RIGHT JOIN Orders as b
ON a.product_id = b.product_id
WHERE MONTH(b.order_date)=2 AND YEAR(b.order_date)=2020
GROUP BY a.product_name
HAVING unit >= 100
```

1378. Replace Employee ID With The Unique Identifier

```sql
select unique_id, name
from employees as emp
left join employeeUNI as uni
on uni.id = emp.id
```

1407. Top Travellers

```sql
SELECT name,sum(
	if(distance is NULL,0,distance)
	) as travelled_distance
FROM Users u
LEFT JOIN Rides r ON u.id=r.user_id
GROUP BY User_id
ORDER BY travelled_distance desc,name asc
```

1484. Group Sold Products By The Date

```sql
select 
sell_date, count(distinct product) as num_sold, 
group_concat(distinct product) as products 
from activities 
group by sell_date;
```

1517. Find Users With Valid E-Mails

```sql
select * from Users 
where right(mail, 13) like "@leetcode.com"
and left(mail, length(mail)-13) 
rlike '^[A-Za-z][A-Za-z0-9._-]*$'
```

1527. Patients With a Condition

```sql
select patient_id, patient_name, conditions 
from patients 
where conditions 
like 'DIAB10%' or 
conditions like '% DIAB10%';
```

1581. Customer Who Visited but Did Not Make Any Transactions

```sql
SELECT customer_id, COUNT(Visits.visit_id) AS count_no_trans
FROM Visits LEFT JOIN Transactions ON Visits.visit_id = Transactions.visit_id
WHERE transaction_id IS NULL
GROUP BY customer_id;
```

1587. Bank Account Summary II

```sql
select name, sum(amount) as balance
from Transactions as t
inner join Users as u 
on t.account = u.account
group by t.account
having sum(amount) > 10000
```

1633. Percentage of Users Attended a Contest

```sql
SELECT contest_id, (
    ROUND((COUNT(DISTINCT(user_id)) / (
        SELECT COUNT(user_name) FROM Users
    )) * 100 , 2)
) AS percentage
FROM Register
GROUP BY contest_id
ORDER BY 2 DESC, 1 ASC
```

1661. Average Time of Process per Machine

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

1661. Average Time of Process per Machine

```sql
Not done yet
```

1667. Fix Names in a Table

```sql
select user_id, CONCAT(UPPER(LEFT(name, 1)), LOWER(RIGHT(name, LENGTH(name) - 1))) as name
from Users
order by user_id
```

1683. Invalid Tweets

```sql
select tweet_id
from Tweets
where length(content) > 15;
```

1693. Daily Leads and Partners

```sql
select date_id, make_name, count(distinct lead_id) as unique_leads, count(distinct partner_id) as unique_partners
from DailySales
group by date_id, make_name
```

1729. Find Followers Count

```sql
select user_id, count(follower_id) followers_count from followers
group by user_id
order by user_id
```

1731. The Number of Employees Which Report to Each Employee

```sql
select e1.employee_id, e1.name, count(e2.employee_id) as reports_count, round(avg(e2.age)) as average_age
from Employees e1, Employees e2
where e1.employee_id = e2.reports_to
group by 1
having reports_count>0
order by 1
```

1741. Find Total Time Spent by Each Employee

```sql
SELECT
    event_day as day,
    emp_id,
    SUM(out_time - in_time) as total_time
FROM Employees
GROUP BY event_day, emp_id
```

1757. Recyclable and Low Fat Products

```sql
select product_id
from Products 
where low_fats  = 'Y' AND recyclable = 'Y';
```

1789. Primary Department for Each Employee

```sql
WITH CTE AS(
SELECT Employee_id, count(department_id) as cnt
FROM Employee
GROUP BY employee_id
)

SELECT employee_id, department_id
FROM Employee
WHERE employee_id IN (SELECT Employee_id FROM CTE WHERE cnt = 1)
UNION
SELECT employee_id, department_id
FROM Employee
WHERE primary_flag = "Y"
```

1795. Rearrange Products Table

```sql
select product_id, "store1" as store, store1 as price
from Products 
where store1 is not null

union

select product_id, "store2" as store, store2 as price
from Products 
where store2 is not null

union

select product_id, "store3" as store, store3 as price
from Products 
where store3 is not null
```

1873. Calculate Special Bonus

```sql
SELECT employee_id, salary*(employee_id%2)*(name NOT LIKE "M%") as "bonus"
FROM Employees
ORDER BY employee_id
```

1890. The Latest Login in 2020

```sql
select user_id, max(time_stamp) as last_stamp
from Logins
where year(time_stamp) = 2020
group by user_id;
```

1965. Employees With Missing Information

```sql
(SELECT E.employee_id
FROM Employees E
LEFT JOIN Salaries S on E.employee_id = S.employee_id
WHERE S.salary IS NULL
UNION

SELECT S.employee_id
FROM Employees E
RIGHT JOIN Salaries S on E.employee_id = S.employee_id
WHERE E.name IS NULL)
ORDER BY employee_id
```

1978. Employees Whose Manager Left the Company

```sql
SELECT employee_id
FROM Employees as a
WHERE manager_id not in (SELECT employee_id FROM employees) and salary < 30000
ORDER BY employee_id ASC
```

2356. Number of Unique Subjects Taught by Each Teacher

```sql
select teacher_id, count(distinct(subject_id)) as cnt
from teacher
group by teacher_id
```