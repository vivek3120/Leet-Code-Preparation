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



SELECT 
    user_id,
    COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id ASC;

