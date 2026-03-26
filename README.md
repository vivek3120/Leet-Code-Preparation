# Leet-Code-Preparation SQL

### Combine 2 tables
select p.firstName, p.lastName, a.city, a.state from person p left join Address a on a.personid=p.personid
________________________________________________________________________________________________

### 181. Employees Earning More Than Their Managers
select e.name as Employee from Employee e join Employee m on e.managerid=m.id where m.salary<e.salary ;
_________________________________________________________________________________________________

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

_____________________________________________________________________________________________________

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
______________________________________________________________________________________________________

### 511. Game Play Analysis I
select player_id, first_login  from(select player_id, event_date as first_login,
row_number() over (partition by player_id  order by event_date asc) as row_num
 from activity ) a
 where row_num=1
 _____________________________________________________________________________________________________

 ### 1661. Average Time of Process per Machine

 select a.machine_id,round(avg(b.timestamp-a.timestamp),3) as processing_time
FROM Activity a
JOIN Activity b

    ON a.machine_id = b.machine_id
   AND a.process_id = b.process_id
   AND a.activity_type = 'start'
   AND b.activity_type = 'end'
GROUP BY a.machine_id;
_________________________________________________________________________________________________________
### 1667. Fix Names in a Table

select user_id, 
 UPPER(LEFT(name, 1)) + LOWER(SUBSTRING(name, 2, LEN(name))) AS name
from users
order by user_id
_________________________________________________________________________________________________________
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
