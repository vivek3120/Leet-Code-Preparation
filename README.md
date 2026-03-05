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
