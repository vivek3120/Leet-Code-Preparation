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




SELECT 
    user_id,
    COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id ASC;

