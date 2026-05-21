# Leet-Code-Preparation SQL

### Combine 2 tables
select p.firstName, p.lastName, a.city, a.state from person p left join Address a on a.personid=p.personid
__________________________________________________________________________________________________

### 181. Employees Earning More Than Their Managers
select e.name as Employee from Employee e join Employee m on e.managerid=m.id where m.salary<e.salary ;
___________________________________________________________________________________________________

### 1204. Last Person to Fit in the Bus
SELECT top 1 person_name
FROM ( SELECT  person_name, SUM(weight) OVER (ORDER BY turn) AS running_weight
  FROM Queue
) t
WHERE running_weight <= 1000
ORDER BY running_weight DESC

__________________________________________________________________________________________________

### 1045. Customers Who Bought All Products
SELECT customer_id FROM Customer GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) =  (SELECT COUNT(*) FROM Product);

___________________________________________________________________________________________________

### 1251. Average Selling Price
SELECT 
    p.product_id,
    ROUND(
        ISNULL(
            SUM(p.price * u.units) * 1.0 
            / NULLIF(SUM(u.units), 0),
        0),
    2) AS average_price
FROM Prices p
LEFT JOIN UnitsSold u
    ON p.product_id = u.product_id
    AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id;

_____________________________________________________________________________________________________

### 1731. The Number of Employees Which Report to Each Employee
select e.employee_id, e.name,count(f.reports_to) as reports_count,floor(avg(f.age+0.5)) as average_age from employees e join employees f on e.employee_id=f.reports_to  
group by e.employee_id, e.name order by e.employee_id

_____________________________________________________________________________________________________

### 3793. Find Users with High Token Usage
SELECT 
    p.user_id,
    COUNT(*) AS prompt_count,
    ROUND(AVG(CAST(tokens AS DECIMAL(10,2))), 2) AS avg_tokens
FROM prompts p
GROUP BY p.user_id
HAVING 
    COUNT(*) >= 3
    AND MAX(p.tokens) > AVG(p.tokens)
ORDER BY 
    avg_tokens DESC,
    user_id ASC;
____________________________________________________________________________________________________

### 511. Game Play Analysis I
select player_id, first_login  from(select player_id, event_date as first_login,
row_number() over (partition by player_id  order by event_date asc) as row_num
 from activity ) a
 where row_num=1
 ___________________________________________________________________________________________________

 ### 1661. Average Time of Process per Machine

 select a.machine_id,round(avg(b.timestamp-a.timestamp),3) as processing_time
FROM Activity a
JOIN Activity b

    ON a.machine_id = b.machine_id
   AND a.process_id = b.process_id
   AND a.activity_type = 'start'
   AND b.activity_type = 'end'
GROUP BY a.machine_id;
_______________________________________________________________________________________________________
### 1667. Fix Names in a Table

select user_id, 
 UPPER(LEFT(name, 1)) + LOWER(SUBSTRING(name, 2, LEN(name))) AS name
from users
order by user_id
________________________________________________________________________________________________________
### 1633. Percentage of Users Attended a Contest

SELECT 
    r.contest_id,
    ROUND(COUNT(r.user_id) * 100.0 / 
          (SELECT COUNT(*) FROM Users), 2) AS percentage
FROM Register r
GROUP BY r.contest_id
ORDER BY percentage DESC, r.contest_id ASC;
_________________________________________________________________________________________________________
### 3475. DNA Pattern Recognition

select sample_id , dna_sequence , species , 
CASE 
        WHEN dna_sequence LIKE 'ATG%' THEN 1 
        ELSE 0 
    END AS has_start, 
    CASE 
        WHEN dna_sequence LIKE '%TAA'
          OR dna_sequence LIKE '%TAG'
          OR dna_sequence LIKE '%TGA'
        THEN 1 
        ELSE 0 
    END AS has_stop,   
    CASE 
        WHEN dna_sequence LIKE '%ATAT%' THEN 1 
        ELSE 0 
    END AS has_atat, 
    CASE 
        WHEN dna_sequence LIKE '%GGG%' THEN 1 
        ELSE 0 
    END AS has_ggg
FROM Samples
ORDER BY sample_id;
_________________________________________________________________________________________________________
### 177. Nth Highest Salary
CREATE FUNCTION getNthHighestSalary(@N INT) RETURNS INT AS
BEGIN
    RETURN (
        /* Write your T-SQL query statement below. */
        select max(salary)
        from(
select salary, dense_rank() over (order by salary desc) as dns from Employee
        ) d 
        where dns=@N
    );
END
____________________________________________________________________________________________________________
### 1873. Calculate Special Bonus
SELECT 
    employee_id,
    CASE 
        WHEN employee_id % 2 = 1 AND name NOT LIKE 'M%' 
        THEN salary
        ELSE 0
    END AS bonus
FROM Employees
ORDER BY employee_id;
______________________________________________________________________________________________________________
### 196. Delete Duplicate Emails
DELETE p1
FROM Person p1
JOIN Person p2
  ON p1.email = p2.email
 AND p1.id > p2.id;
 ____________________________________________________________________________________________________________
### 180. Consecutive Numbers
select distinct consecutivenums from(select num as consecutivenums,
lead(num,1) over (order by id asc) as lnum1,
lead(num,2) over (order by id asc) as lnum2
 from logs
) k where consecutivenums =lnum1
and consecutivenums =lnum2
_____________________________________________________________________________________________________________
### 584. Find Customer Referee
select name from Customer where referee_id !=2 or referee_id is null
_____________________________________________________________________________________________________________
### 586. Customer Placing the Largest Number of Orders
SELECT top 1 customer_number
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(*) DESC
;
_____________________________________________________________________________________________________________
### 607. Sales Person
SELECT s.name
FROM SalesPerson s
WHERE NOT EXISTS (
    SELECT 1
    FROM Orders o
    JOIN Company c ON c.com_id = o.com_id
    WHERE o.sales_id = s.sales_id
      AND c.name = 'RED'
);
______________________________________________________________________________________________________________
### 595. Big Countries
select name, population, area from World where
area>= 3000000
or population>= 25000000
____________________________________________________________________________________________________________
### 175. Combine Two Tables
select p.firstName, p.lastName, a.city, a.state from person p left join Address a on a.personid=p.personid
_____________________________________________________________________________________________________________
### 596. Classes With at Least 5 Students
select class from courses
group by class
having count(class)>=5
______________________________________________________________________________________________________________
### 577. Employee Bonus
select e.name, b.bonus  from Employee e left join bonus b
on e.empid=b.empid where b.bonus<1000
or b.bonus is null
______________________________________________________________________________________________________________
### 610. Triangle Judgement
select x,y,z,
    CASE
        WHEN x + y > z 
         AND x + z > y 
         AND y + z > x
        THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM Triangle;
_____________________________________________________________________________________________________________
### 183. Customers Who Never Order
select c.name as Customers from Customers c left join Orders o on c.id=o.customerId where o.customerid is null
______________________________________________________________________________________________________________
### 197. Rising Temperature
select w.id from Weather w join weather e on e.id=e.id
where w.recorddate= DATEADD(day, 1, e.recordDate)
and w.temperature>e.temperature
______________________________________________________________________________________________________________
### 1075. Project Employees I
select p.project_id, round(avg(1.00*e.experience_years),2)  as average_years from project p join employee e on e.employee_id=p.employee_id
group by p.project_id
order by p.project_id
_______________________________________________________________________________________________________________
### 610. Triangle Judgement
select x,y,z,
    CASE
        WHEN x + y > z 
         AND x + z > y 
         AND y + z > x
        THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM Triangle;
_________________________________________________________________________________________________________________
### 1280. Students and Examinations
select s.student_id, s.student_name, su.subject_name,
 count(e.subject_name) as attended_exams from Students s 
 CROSS JOIN Subjects su
LEFT JOIN Examinations e
    ON e.student_id = s.student_id
    AND e.subject_name = su.subject_name
group by s.student_id, s.student_name, su.subject_name
order by student_id, subject_name
_________________________________________________________________________________________________________________
### 1527. Patients With a Condition
select patient_id , patient_name , conditions from patients
where conditions like '% DIAB1%'
or conditions like 'DIAB1%'
_________________________________________________________________________________________________________________
### 2356. Number of Unique Subjects Taught by Each Teacher
select teacher_id,count(distinct subject_id)as cnt from teacher 
group by teacher_id
_________________________________________________________________________________________________________________
### 1327. List the Products Ordered in a Period
select p.product_name, SUM(o.unit) AS unit from products p
join orders o on o.product_id=p.product_id
where order_date>'2020-01-31' and order_date<'2020-03-01'
group by product_name having SUM(o.unit)>=100
________________________________________________________________________________________________________________
### 1693. Daily Leads and Partners
select date_id, make_name,count(distinct lead_id)  as unique_leads,count(distinct partner_id)   as unique_partners from dailysales group by date_id,make_name
_______________________________________________________________________________________________________________
### 178. Rank Scores
select score, dense_rank() over(order by score desc) as rank from Scores
________________________________________________________________________________________________________________
### 1393. Capital Gain/Loss
select stock_name,
sum(case when operation='Sell' then price
when operation='Buy' then -price
end)
as capital_gain_loss from stocks
group by stock_name
________________________________________________________________________________________________________________
### 1907. Count Salary Categories
SELECT 
    'Low Salary' AS category,
    SUM(CASE WHEN income < 20000 THEN 1 ELSE 0 END) AS accounts_count
FROM Accounts
UNION ALL
SELECT 
    'Average Salary',
    SUM(CASE WHEN income BETWEEN 20000 AND 50000 THEN 1 ELSE 0 END)
FROM Accounts
UNION ALL
SELECT 
    'High Salary',
    SUM(CASE WHEN income > 50000 THEN 1 ELSE 0 END)
FROM Accounts;
______________________________________________________________________________________________________________
### 1158. Market Analysis I
select u.user_id as buyer_id,u.join_date, count(o.order_id) as orders_in_2019 from 
users u left join orders o on u.user_id = o.buyer_id 
 and o.order_date >= '2019-01-01'
   AND o.order_date < '2020-01-01'
GROUP BY u.user_id, u.join_date;
_______________________________________________________________________________________________________________
### 184. Department Highest Salary
SELECT
    d.name AS Department,
    e.name AS Employee,
    e.salary AS Salary
FROM Employee e
JOIN Department d
  ON e.departmentId = d.id
JOIN (
    SELECT departmentId, MAX(salary) AS max_salary
    FROM Employee
    GROUP BY departmentId
) m
  ON e.departmentId = m.departmentId
 AND e.salary = m.max_salary;
_______________________________________________________________________________________________________________
### 1164. Product Price at a Given Date
WITH ranked_prices AS (
    SELECT
        product_id,
        new_price,
        change_date,
        ROW_NUMBER() OVER (
            PARTITION BY product_id
            ORDER BY change_date DESC
        ) AS rn
    FROM Products
    WHERE change_date <= '2019-08-16'
)
SELECT
    p.product_id,
    COALESCE(r.new_price, 10) AS price
FROM (SELECT DISTINCT product_id FROM Products) p
LEFT JOIN ranked_prices r
    ON p.product_id = r.product_id
   AND r.rn = 1;
_________________________________________________________________________________________________________________
### 1978. Employees Whose Manager Left the Company
SELECT e.employee_id FROM Employees e
WHERE e.salary < 30000
  AND e.manager_id IS NOT NULL
  AND NOT EXISTS (
      SELECT 1
      FROM Employees m
      WHERE m.employee_id = e.manager_id
  )
ORDER BY e.employee_id;
_________________________________________________________________________________________________________________
### 1407. Top Travellers
select u.name, COALESCE(SUM(r.distance), 0) AS travelled_distance
 from users u left join 
rides r on r.user_id= u.id 
group by u.name , u.id
order by travelled_distance desc, u.name asc
__________________________________________________________________________________________________________________
### 3436. Find Valid Emails
SELECT user_id, email
FROM Users
WHERE email REGEXP '^[A-Za-z0-9_]+@[A-Za-z]+\\.com$'
ORDER BY user_id;
___________________________________________________________________________________________________________________
### 1517. Find Users With Valid E-Mails
SELECT user_id, name, mail FROM Users
WHERE RIGHT(mail, 13) COLLATE Latin1_General_CS_AS like '@leetcode.com'
AND LEFT(mail, 1) LIKE '[a-zA-Z]'
AND LEFT(mail, LEN(mail) - 13) NOT LIKE '%[^a-zA-Z0-9_.-]%';
___________________________________________________________________________________________________________________
### 3465. Find Products with Valid Serial Numbers
SELECT
    p.product_id,
    p.product_name,
    p.description
FROM products p
WHERE
       p.description COLLATE Latin1_General_CS_AS LIKE 'SN[0-9][0-9][0-9][0-9]-[0-9][0-9][0-9][0-9][^A-Za-z0-9]%'
    OR p.description COLLATE Latin1_General_CS_AS LIKE '%[^A-Za-z0-9]SN[0-9][0-9][0-9][0-9]-[0-9][0-9][0-9][0-9][^A-Za-z0-9]%'
    OR p.description COLLATE Latin1_General_CS_AS LIKE '%[^A-Za-z0-9]SN[0-9][0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]'
    OR p.description COLLATE Latin1_General_CS_AS LIKE 'SN[0-9][0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]'
ORDER BY p.product_id;
_______________________________________________________________________________________________________________________
### 176. Second Highest Salary
select max(salary) as secondhighestsalary 
from   employee 
where salary<(select max(salary) from employee)
_____________________________________________________________________________________________________________________
### 619. Biggest Single Number
SELECT MAX(num) AS num
FROM mynumbers
WHERE num IN (
    SELECT num
    FROM mynumbers
    GROUP BY num
    HAVING COUNT(*) = 1
);
_____________________________________________________________________________________________________________________
### 3220. Odd and Even Transactions
select transaction_date,
    SUM(CASE WHEN amount % 2 = 1 THEN amount ELSE 0 END) AS odd_sum,
    SUM(CASE WHEN amount % 2 = 0 THEN amount ELSE 0 END) AS even_sum
 from transactions 
 group by transaction_date
 order by transaction_date asc
_____________________________________________________________________________________________________________________
### 1193. Monthly Transactions I
select  FORMAT(trans_date, 'yyyy-MM') AS month,country,count(*) as trans_count,
sum(case when state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
sum(amount) as trans_total_amount,
SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
from transactions
GROUP BY FORMAT(trans_date, 'yyyy-MM'), country;
____________________________________________________________________________________________________________________
### 3497. Analyze Subscription Conversion 
select user_id,
round(avg(case when activity_type='free_trial' then activity_duration*1.0 end),2)  as trial_avg_duration, 
round(avg(case when activity_type='paid' then activity_duration*1.0  end),2)  as paid_avg_duration from useractivity
group by user_id
HAVING 
SUM(CASE WHEN activity_type = 'free_trial' THEN 1 ELSE 0 END) > 0
AND SUM(CASE WHEN activity_type = 'paid' THEN 1 ELSE 0 END) > 0
order by user_id
___________________________________________________________________________________________________________________
### 608. Tree Node
select id,
case when p_id is null then 'Root'
when id in (select distinct p_id from tree) then 'Inner'
else 'Leaf'
 end as type 
 from tree
__________________________________________________________________________________________________________________
### 602. Friend Requests II: Who Has the Most Friends
SELECT top 1 id, COUNT(*) AS num
FROM
(SELECT requester_id AS id FROM RequestAccepted
 UNION ALL SELECT accepter_id AS id FROM RequestAccepted
) t
GROUP BY id
ORDER BY num DESC;
_________________________________________________________________________________________________________________
### 626. Exchange Seats
SELECT 
CASE
WHEN id % 2 = 1 AND id + 1 <= (SELECT MAX(id) FROM Seat) THEN id + 1
WHEN id % 2 = 0 THEN id - 1
ELSE id
END AS id,student
FROM Seat ORDER BY id;
_________________________________________________________________________________________________________________
### 1174. Immediate Food Delivery II
SELECT 
ROUND(100.0 * AVG(
CASE WHEN d.order_date = d.customer_pref_delivery_date THEN 1.0
ELSE 0.0 END),2) AS immediate_percentage
FROM Delivery d
JOIN (
SELECT customer_id, MIN(order_date) AS first_order_date
FROM Delivery
GROUP BY customer_id
) f
ON d.customer_id = f.customer_id
AND d.order_date = f.first_order_date;
_________________________________________________________________________________________________________________
### 3564. Seasonal Sales Analysis
WITH category_sales AS (
SELECT CASE
WHEN MONTH(s.sale_date) IN (12, 1, 2) THEN 'Winter'
WHEN MONTH(s.sale_date) IN (3, 4, 5) THEN 'Spring'
WHEN MONTH(s.sale_date) IN (6, 7, 8) THEN 'Summer'
WHEN MONTH(s.sale_date) IN (9, 10, 11) THEN 'Fall'
END AS season,
p.category,
SUM(s.quantity) AS total_quantity,
SUM(s.quantity * s.price) AS total_revenue
FROM sales s
JOIN products p
ON s.product_id = p.product_id
GROUP BY
CASE
WHEN MONTH(s.sale_date) IN (12, 1, 2) THEN 'Winter'
WHEN MONTH(s.sale_date) IN (3, 4, 5) THEN 'Spring'
WHEN MONTH(s.sale_date) IN (6, 7, 8) THEN 'Summer'
WHEN MONTH(s.sale_date) IN (9, 10, 11) THEN 'Fall'
END,
p.category
),
ranked AS (
SELECT *,
ROW_NUMBER() OVER (
PARTITION BY season
ORDER BY total_quantity DESC,
total_revenue DESC,
category ASC
) AS rn
FROM category_sales
)
SELECT season,category,total_quantity,total_revenue
FROM ranked
WHERE rn = 1
ORDER BY season;
_______________________________________________________________________________________________________________
### 3521. Find Product Recommendation Pairs
SELECT
p1.product_id AS product1_id,
p2.product_id AS product2_id,
i1.category AS product1_category,
i2.category AS product2_category,
COUNT(DISTINCT p1.user_id) AS customer_count
FROM ProductPurchases p1
JOIN ProductPurchases p2
ON p1.user_id = p2.user_id
AND p1.product_id < p2.product_id
JOIN ProductInfo i1
ON p1.product_id = i1.product_id
JOIN ProductInfo i2
ON p2.product_id = i2.product_id
GROUP BY
p1.product_id,
p2.product_id,
i1.category,
i2.category
HAVING COUNT(DISTINCT p1.user_id) >= 3
ORDER BY
customer_count DESC,
product1_id ASC,
product2_id ASC;
______________________________________________________________________________________________________________
### 1934. Confirmation Rate
SELECT
s.user_id,
ROUND(AVG(CASE WHEN c.action = 'confirmed' THEN 1.00
 ELSE 0.00 END),2) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c
ON s.user_id = c.user_id
GROUP BY s.user_id;
_________________________________________________________________________________________________
### 1321. Restaurant Growth
WITH daily_amount AS (
    SELECT
        visited_on,
        SUM(amount) AS amount
    FROM Customer
    GROUP BY visited_on
),
moving_amount AS (
    SELECT
        visited_on,
        SUM(amount) OVER (
            ORDER BY visited_on
            ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
        ) AS amount,
        ROUND(
            SUM(amount) OVER (
                ORDER BY visited_on
                ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
            ) / 7.0,
            2
        ) AS average_amount,
        COUNT(*) OVER (
            ORDER BY visited_on
            ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
        ) AS day_count
    FROM daily_amount
)
SELECT
    visited_on,
    amount,
    average_amount
FROM moving_amount
WHERE day_count = 7
ORDER BY visited_on;
__________________________________________________________________________________________________________
### 3580. Find Consistently Improving Employees
WITH ranked_reviews AS (
SELECT
employee_id,
review_date,
rating,
ROW_NUMBER() OVER (
PARTITION BY employee_id
ORDER BY review_date DESC
) AS rn
FROM performance_reviews
),
last_three AS (
SELECT
employee_id,
review_date,
rating,
ROW_NUMBER() OVER (
PARTITION BY employee_id
ORDER BY review_date ASC
) AS chronological_rn
FROM ranked_reviews
WHERE rn <= 3
),
pivoted AS (
SELECT
employee_id,
MAX(CASE WHEN chronological_rn = 1 THEN rating END) AS first_rating,
MAX(CASE WHEN chronological_rn = 2 THEN rating END) AS second_rating,
MAX(CASE WHEN chronological_rn = 3 THEN rating END) AS third_rating,
COUNT(*) AS review_count
FROM last_three
GROUP BY employee_id
)
SELECT
e.employee_id,
e.name,
p.third_rating - p.first_rating AS improvement_score
FROM pivoted p
JOIN employees e
ON p.employee_id = e.employee_id
WHERE p.review_count = 3
AND p.first_rating < p.second_rating
AND p.second_rating < p.third_rating
ORDER BY improvement_score DESC, e.name ASC;
________________________________________________________________________________________________
### 3554. Find Category Recommendation Pairs
WITH user_categories AS (
SELECT DISTINCT
pp.user_id,
pi.category
FROM ProductPurchases pp
JOIN ProductInfo pi
ON pp.product_id = pi.product_id
),
category_pairs AS (
SELECT
uc1.category AS category1,
uc2.category AS category2,
uc1.user_id
FROM user_categories uc1
JOIN user_categories uc2
ON uc1.user_id = uc2.user_id
AND uc1.category < uc2.category
)
SELECT
category1,
category2,
COUNT(DISTINCT user_id) AS customer_count
FROM category_pairs
GROUP BY category1, category2
HAVING COUNT(DISTINCT user_id) >= 3
ORDER BY customer_count DESC, category1 ASC, category2 ASC;
______________________________________________________________________________________________________
### 185. Department Top Three Salaries
WITH ranked_salary AS (
SELECT
e.id,
e.name AS employee,
e.salary,
e.departmentId,
DENSE_RANK() OVER (
PARTITION BY e.departmentId
ORDER BY e.salary DESC
) AS salary_rank
FROM Employee e
)
SELECT
d.name AS Department,
r.employee AS Employee,
r.salary AS Salary
FROM ranked_salary r
JOIN Department d
ON r.departmentId = d.id
WHERE r.salary_rank <= 3;
______________________________________________________________________________________________________
### 3832. Find Users with Persistent Behavior Patterns
WITH one_action_days AS (
    SELECT
        user_id,
        action_date,
        MIN(action) AS action
    FROM activity
    GROUP BY user_id, action_date
    HAVING COUNT(*) = 1
),
grouped_days AS (
    SELECT
        user_id,
        action,
        action_date,
        DATEADD(
            DAY,
            -ROW_NUMBER() OVER (
                PARTITION BY user_id, action
                ORDER BY action_date
            ),
            action_date
        ) AS grp
    FROM one_action_days
),
streaks AS (
    SELECT
        user_id,
        action,
        COUNT(*) AS streak_length,
        MIN(action_date) AS start_date,
        MAX(action_date) AS end_date
    FROM grouped_days
    GROUP BY user_id, action, grp
    HAVING COUNT(*) >= 5
),
ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY user_id
            ORDER BY streak_length DESC, start_date ASC
        ) AS rn
    FROM streaks
)
SELECT
    user_id,
    action,
    streak_length,
    start_date,
    end_date
FROM ranked
WHERE rn = 1
ORDER BY streak_length DESC, user_id ASC;
__________________________________________________________________________________________________
### 3374. First Letter Capitalization II

__________________________________________________________________________________________________
### 3451. Find Invalid IP Addresses
SELECT
ip,
COUNT(*) AS invalid_count
FROM logs
WHERE ip NOT REGEXP '^((25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])\\.){3}(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])$'
GROUP BY ip
ORDER BY invalid_count DESC, ip DESC;
________________________________________________________________________________________________
### 3673. Find Zombie Sessions
WITH session_summary AS (
SELECT
session_id,
user_id,
DATEDIFF(
MINUTE,
MIN(event_timestamp),
MAX(event_timestamp)
) AS session_duration_minutes,

SUM(CASE WHEN event_type = 'scroll' THEN 1 ELSE 0 END) AS scroll_count,
SUM(CASE WHEN event_type = 'click' THEN 1 ELSE 0 END) AS click_count,
SUM(CASE WHEN event_type = 'purchase' THEN 1 ELSE 0 END) AS purchase_count
FROM app_events
GROUP BY session_id, user_id
)
SELECT
session_id,
user_id,
session_duration_minutes,
scroll_count
FROM session_summary
WHERE session_duration_minutes > 30
AND scroll_count >= 5
AND CAST(click_count AS DECIMAL(10, 2)) / scroll_count < 0.20
AND purchase_count = 0
ORDER BY scroll_count DESC, session_id ASC;
__________________________________________________________________________________________________
### 601. Human Traffic of Stadium
WITH filtered AS (
SELECT
id,
visit_date,
people,
id - ROW_NUMBER() OVER (ORDER BY id) AS grp
FROM Stadium
WHERE people >= 100
),
valid_groups AS (
SELECT
grp
FROM filtered
GROUP BY grp
HAVING COUNT(*) >= 3
)
SELECT
f.id,
f.visit_date,
f.people
FROM filtered f
JOIN valid_groups v
ON f.grp = v.grp
ORDER BY f.visit_date ASC;
__________________________________________________________________________________________________
### 3482. Analyze Organization Hierarchy
WITH hierarchy AS (
    SELECT
        employee_id,
        employee_name,
        manager_id,
        salary,
        1 AS level
    FROM Employees
    WHERE manager_id IS NULL

    UNION ALL

    SELECT
        e.employee_id,
        e.employee_name,
        e.manager_id,
        e.salary,
        h.level + 1 AS level
    FROM Employees e
    INNER JOIN hierarchy h
        ON e.manager_id = h.employee_id
),

subordinates AS (
    SELECT
        employee_id AS manager_id,
        employee_id AS employee_id,
        salary
    FROM Employees

    UNION ALL

    SELECT
        s.manager_id,
        e.employee_id,
        e.salary
    FROM subordinates s
    INNER JOIN Employees e
        ON e.manager_id = s.employee_id
)

SELECT
    h.employee_id,
    h.employee_name,
    h.level,
    COUNT(s.employee_id) - 1 AS team_size,
    SUM(s.salary) AS budget
FROM hierarchy h
INNER JOIN subordinates s
    ON h.employee_id = s.manager_id
GROUP BY
    h.employee_id,
    h.employee_name,
    h.level
ORDER BY
    h.level ASC,
    budget DESC,
    h.employee_name ASC
OPTION (MAXRECURSION 0);
___________________________________________________________________________________________________
### 550. Game Play Analysis IV
WITH first_login AS (
SELECT
player_id,
MIN(event_date) AS first_date
FROM Activity
GROUP BY player_id
)
SELECT
ROUND(
CAST(COUNT(a.player_id) AS FLOAT) / COUNT(f.player_id),
2
) AS fraction
FROM first_login f
LEFT JOIN Activity a
ON f.player_id = a.player_id
AND a.event_date = DATEADD(DAY, 1, f.first_date);
__________________________________________________________________________________________________
### 570. Managers with at Least 5 Direct Reports
SELECT m.name FROM Employee e
JOIN Employee m ON e.managerId = m.id
GROUP BY m.id, m.name
HAVING COUNT(e.id) >= 5;
_______________________________________________________________________________________________________
### 585. Investments in 2016
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM (
    SELECT
        pid,
        tiv_2015,
        tiv_2016,
        lat,
        lon,
        COUNT(*) OVER (PARTITION BY tiv_2015) AS same_tiv_2015_count,
        COUNT(*) OVER (PARTITION BY lat, lon) AS same_location_count
    FROM Insurance
) x
WHERE same_tiv_2015_count > 1
  AND same_location_count = 1;
___________________________________________________________________________________________________
### 1341. Movie Rating
SELECT results
FROM
(
    SELECT TOP 1
        u.name AS results,
        1 AS sort_order
    FROM MovieRating mr
    JOIN Users u
        ON mr.user_id = u.user_id
    GROUP BY u.user_id, u.name
    ORDER BY COUNT(*) DESC, u.name ASC
) A

UNION ALL

SELECT results
FROM
(
    SELECT TOP 1
        m.title AS results,
        2 AS sort_order
    FROM MovieRating mr
    JOIN Movies m
        ON mr.movie_id = m.movie_id
    WHERE mr.created_at >= '2020-02-01'
      AND mr.created_at < '2020-03-01'
    GROUP BY m.movie_id, m.title
    ORDER BY AVG(CAST(mr.rating AS FLOAT)) DESC, m.title ASC
) B;
__________________________________________________________________________________________________________
### 3626. Find Stores with Inventory Imbalance
WITH ranked_inventory AS (
    SELECT
        i.store_id,
        i.product_name,
        i.quantity,
        i.price,
        COUNT(*) OVER (PARTITION BY i.store_id) AS product_count,

        ROW_NUMBER() OVER (
            PARTITION BY i.store_id
            ORDER BY i.price DESC, i.product_name
        ) AS expensive_rank,

        ROW_NUMBER() OVER (
            PARTITION BY i.store_id
            ORDER BY i.price ASC, i.product_name
        ) AS cheapest_rank
    FROM inventory i
),
most_expensive AS (
    SELECT
        store_id,
        product_name AS most_exp_product,
        quantity AS most_exp_quantity
    FROM ranked_inventory
    WHERE expensive_rank = 1
),
cheapest AS (
    SELECT
        store_id,
        product_name AS cheapest_product,
        quantity AS cheapest_quantity,
        product_count
    FROM ranked_inventory
    WHERE cheapest_rank = 1
)
SELECT
    s.store_id,
    s.store_name,
    s.location,
    me.most_exp_product,
    c.cheapest_product,
    ROUND(
        CAST(c.cheapest_quantity AS DECIMAL(10, 2)) 
        / me.most_exp_quantity,
        2
    ) AS imbalance_ratio
FROM stores s
JOIN most_expensive me
    ON s.store_id = me.store_id
JOIN cheapest c
    ON s.store_id = c.store_id
WHERE c.product_count >= 3
  AND me.most_exp_quantity < c.cheapest_quantity
ORDER BY imbalance_ratio DESC, s.store_name ASC;
_______________________________________________________________________________________________
### 3716. Find Churn Risk Customers
WITH user_events AS (
    SELECT
        user_id,
        event_date,
        event_type,
        plan_name,
        monthly_amount,

        ROW_NUMBER() OVER (
            PARTITION BY user_id
            ORDER BY event_date DESC, event_id DESC
        ) AS rn,

        MIN(event_date) OVER (
            PARTITION BY user_id
        ) AS first_event_date,

        MAX(event_date) OVER (
            PARTITION BY user_id
        ) AS last_event_date,

        MAX(monthly_amount) OVER (
            PARTITION BY user_id
        ) AS max_historical_amount,

        SUM(CASE WHEN event_type = 'downgrade' THEN 1 ELSE 0 END) OVER (
            PARTITION BY user_id
        ) AS downgrade_count
    FROM subscription_events
)

SELECT
    user_id,
    plan_name AS current_plan,
    monthly_amount AS current_monthly_amount,
    max_historical_amount,
    DATEDIFF(day, first_event_date, last_event_date) AS days_as_subscriber
FROM user_events
WHERE rn = 1
  AND event_type <> 'cancel'
  AND downgrade_count >= 1
  AND monthly_amount < max_historical_amount * 0.50
  AND DATEDIFF(day, first_event_date, last_event_date) >= 60
ORDER BY days_as_subscriber DESC, user_id ASC;
___________________________________________________________________________________
### 3586. Find COVID Recovery Patients
WITH first_positive AS (
    SELECT
        patient_id,
        MIN(test_date) AS first_positive_date
    FROM covid_tests
    WHERE result = 'Positive'
    GROUP BY patient_id
),
first_negative_after_positive AS (
    SELECT
        fp.patient_id,
        fp.first_positive_date,
        MIN(ct.test_date) AS first_negative_date
    FROM first_positive fp
    JOIN covid_tests ct
        ON fp.patient_id = ct.patient_id
       AND ct.result = 'Negative'
       AND ct.test_date > fp.first_positive_date
    GROUP BY fp.patient_id, fp.first_positive_date
)

SELECT
    p.patient_id,
    p.patient_name,
    p.age,
    DATEDIFF(DAY, f.first_positive_date, f.first_negative_date) AS recovery_time
FROM first_negative_after_positive f
JOIN patients p
    ON f.patient_id = p.patient_id
ORDER BY
    recovery_time ASC,
    p.patient_name ASC;
_________________________________________________________________________________________________________________
### 3421. Find Students Who Improved
WITH ranked_scores AS (
    SELECT
        student_id,
        subject,
        score,
        exam_date,
        ROW_NUMBER() OVER (
            PARTITION BY student_id, subject
            ORDER BY exam_date ASC
        ) AS rn_first,
        ROW_NUMBER() OVER (
            PARTITION BY student_id, subject
            ORDER BY exam_date DESC
        ) AS rn_latest
    FROM Scores
),
first_scores AS (
    SELECT
        student_id,
        subject,
        score AS first_score
    FROM ranked_scores
    WHERE rn_first = 1
),
latest_scores AS (
    SELECT
        student_id,
        subject,
        score AS latest_score
    FROM ranked_scores
    WHERE rn_latest = 1
)

SELECT
    f.student_id,
    f.subject,
    f.first_score,
    l.latest_score
FROM first_scores f
JOIN latest_scores l
    ON f.student_id = l.student_id
   AND f.subject = l.subject
WHERE l.latest_score > f.first_score
ORDER BY
    f.student_id ASC,
    f.subject ASC;
________________________________________________________________________________________
### 3611. Find Overbooked Employees
WITH weekly_meetings AS (
    SELECT
        employee_id,
        DATEADD(
            DAY,
            -DATEDIFF(DAY, '19000101', meeting_date) % 7,
            CAST(meeting_date AS DATE)
        ) AS week_start_date,
        SUM(duration_hours) AS total_meeting_hours
    FROM meetings
    GROUP BY
        employee_id,
        DATEADD(
            DAY,
            -DATEDIFF(DAY, '19000101', meeting_date) % 7,
            CAST(meeting_date AS DATE)
        )
),
meeting_heavy AS (
    SELECT
        employee_id,
        COUNT(*) AS meeting_heavy_weeks
    FROM weekly_meetings
    WHERE total_meeting_hours > 20
    GROUP BY employee_id
)

SELECT
    e.employee_id,
    e.employee_name,
    e.department,
    mh.meeting_heavy_weeks
FROM meeting_heavy mh
JOIN employees e
    ON mh.employee_id = e.employee_id
WHERE mh.meeting_heavy_weeks >= 2
ORDER BY
    mh.meeting_heavy_weeks DESC,
    e.employee_name ASC;
