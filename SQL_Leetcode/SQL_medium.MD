176. Second Highest Salary

```sql
SELECT max(e2.salary) AS SecondHighestSalary
FROM employee e1, employee e2
WHERE e2.salary < e1.salary;
```

177. Nth Highest Salary

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
DECLARE M INT;
SET M = N - 1;
  RETURN (
      SELECT DISTINCT Salary FROM Employee ORDER by Salary DESC LIMIT M, 1
  );
END
```

178. Rank Scores

```sql
select score, 
        dense_rank() over (
            order by score desc
        ) as "rank"
from Scores s
```

180. Consecutive Numbers

```sql
select distinct l1.num as ConsecutiveNums
from logs l1
join logs l2
join logs l3
on
l1.id+1=l2.id and
l1.num=l2.num and
l2.id+1=l3.id and
l3.num=l2.num
```

184. Department Highest Salary

```sql
select d.name as Department ,e.name as Employee,e.salary as Salary from Employee e LEFT JOIN Department d on e.departmentId=d.id where e.salary = (select max(salary) from employee e where e.departmentId=d.Id);
```

550. Game Play Analysis IV

```sql
WITH cte AS (
SELECT player_id, MIN(event_date) as first_login
FROM Activity
GROUP BY player_id
)

SELECT ROUND(SUM(CASE WHEN DATEDIFF(event_date, first_login)=1 THEN 1 ELSE 0  END) / COUNT(DISTINCT cte.player_id), 2) as fraction
FROM Activity as a
JOIN cte 
ON a.player_id = cte.player_id
```

570. Managers with at Least 5 Direct Reports

```sql
select Name
from Employee as t1 
join (select ManagerId
    from Employee
    group by ManagerId
    having count(ManagerId) >= 5) as t2
on t1.Id = t2.ManagerId
```

585. Investments in 2016

```sql
select round(sum(TIV_2016),2) as TIV_2016 from insurance
where TIV_2015 in
(select TIV_2015 from insurance group by TIV_2015 having count(TIV_2015) >1)
and concat(LAT, LON) not in
(select concat(LAT, LON) from insurance group by LAT, LON having count(concat(LAT, LON)) >1)
```

602. Friend Requests II: Who Has the Most Friends

```sql
SELECT id, COUNT(id) AS num
FROM 
    (
    SELECT requester_id AS id
    FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id
    FROM RequestAccepted    
    ) AS totoal_table
GROUP BY id
ORDER BY COUNT(id) DESC
LIMIT 1;
```

608. Tree Node

```sql
select distinct t1.id, (
case 
when t1.p_id is null then 'Root'
when t1.p_id is not null and t2.p_id is not null then 'Inner'
when t1.p_id is not null and t2.p_id is null then 'Leaf'
end) as Type
from tree t1
left join tree t2 on t1.id=t2.p_id
```

626. Exchange Seats

```sql
select
case when mod(id,2)=0 then id-1
when mod(id,2)=1 and id+1 not in (select id from seat) then id
else id+1 end as id,
student
from seat order by id
```

1045. Customers Who Bought All Products

```sql
select customer_id
from Customer
group by customer_id
having count(distinct product_key) = 
    (select count(distinct product_key) from Product);
```

1070. Product Sales Analysis III

```sql
select
    product_id,
    year as first_year,
    quantity,
    price
from Sales
where (product_id, year) in 
    (select product_id, min(year) 
     from Sales 
     group by 1);
```

1158. Market Analysis I

```sql
with ord as (
Select * from Orders where YEAR(order_date) = '2019'
)
Select U.user_id as buyer_id,U.join_date, SUM(CASE WHEN order_date is not null then 1 ELSE 0 END) as orders_in_2019 from Users U
left join ord O on U.user_id = O.buyer_id 
group by U.user_id;
```

1164. Product Price at a Given Date

```sql
select * from (
    select product_id, new_price as price from Products
        where (product_id, change_date) in (
            select product_id, max(change_date) from Products
                where change_date <= '2019-08-16'
                group by product_id
        )
    union
        select distinct product_id, 10 as price from Products
            where product_id not in (
                select product_id from Products
                    where change_date <= '2019-08-16'
            )
    ) union_result;
```

1174. Immediate Food Delivery II

```sql
with base_table as (
select *, rank() over(partition by customer_id order by order_date) as ranking 
from delivery )

select round(avg(case when order_date = customer_pref_delivery_date then 1.00 else 0.00 end)*100, 2) as immediate_percentage 
from base_table
where ranking = 1 
```

1204. Last Person to Fit in the Bus

```sql
with cte as (SELECT person_name,weight,turn,SUM(weight) over (order by turn) as total from Queue)
SELECT person_name
FROM cte
where total <= 1000
order by total DESC
LIMIT 1;
```

1321. Restaurant Growth

```sql
WITH TotalAmount AS (SELECT visited_on,
sum(amount) as total
FROM Customer
GROUP BY visited_on)

SELECT visited_on, amount, average_amount
FROM (SELECT visited_on,
sum(total) over(order by visited_on ASC rows between 6 preceding and current row) AS 'amount',
round(avg(total) over(order by visited_on ASC rows between 6 preceding and current row), 2) AS 'average_amount'
FROM TotalAmount) AS Temp
WHERE DATE_SUB(visited_on, INTERVAL 6 DAY) IN (SELECT visited_on FROM TotalAmount)
ORDER BY visited_on ASC
```

1341. Movie Rating

```sql
select name as results
from (

(select name,count(*) as num
from movierating left join users
on movierating.user_id = users.user_id
group by movierating.user_id
order by num desc, name limit 1 )

union

(select title,avg(rating) as rat
from movierating left join movies
on movierating.movie_id = movies.movie_id
where month(created_at) = 2 and year(created_at) = 2020
group by movierating.movie_id
order by rat desc,title limit 1)
) as a
```

1393. Capital Gain/Loss

```sql
SELECT
    stock_name,
    total_sell - total_buy as capital_gain_loss
FROM (
SELECT
    stock_name,
    SUM(CASE WHEN operation = 'Sell' THEN price ELSE 0 END) as total_sell,
    SUM(CASE WHEN operation = 'Buy' THEN price ELSE 0 END ) as total_buy 
FROM Stocks
GROUP BY 1
) x

```

1907. Count Salary Categories

```sql
select "Low Salary" as category, count(*) as accounts_count from Accounts where income < 20000
union 
select "Average Salary" as category, count(*) as accounts_count from Accounts where 20000 <= income and income <= 50000
union
select "High Salary" as category, count(*) as accounts_count from Accounts where 50000 < income
```

1934. Confirmation Rate

```sql
WITH rate_cte AS
  (SELECT user_id,
          ROUND(SUM(CASE
                        WHEN action='confirmed' THEN 1
                        ELSE 0
                    END) / COUNT(*), 2) AS confirmation_rate
   FROM Confirmations
   GROUP BY user_id
   ORDER BY NULL)

SELECT s.user_id,
       IFNULL(r.confirmation_rate, 0) AS confirmation_rate
FROM Signups s
LEFT JOIN rate_cte r ON s.user_id = r.user_id;
```