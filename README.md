# Leet-Code-Preparation SQL

### Combine 2 tables
select p.firstName, p.lastName, a.city, a.state from person p left join Address a on a.personid=p.personid



SELECT 
    user_id,
    COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id ASC;

