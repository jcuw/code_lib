# Leetcode SQL Questions
# [1939\. Filter by Time Intervals](https://leetcode.com/problems/users-that-actively-request-confirmation-messages/)
```
Write an SQL query to find the IDs of the users that requested a confirmation message **twice** within a 24-hour window. Two messages exactly 24 hours apart are considered to be within the window. The `action` does not affect the answer, only the request time.

Return the result table in **any order**.

Confirmations table:
+---------+---------------------+-----------+
| user_id | time_stamp          | action    |
+---------+---------------------+-----------+
| 3       | 2021-01-06 03:30:46 | timeout   |
| 3       | 2021-01-06 03:37:45 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 11:57:30 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-01-23 00:00:00 | timeout   |
| 6       | 2021-10-23 14:14:14 | confirmed |
| 6       | 2021-10-24 14:14:13 | timeout   |
+---------+---------------------+-----------+
Output: 
+---------+
| user_id |
+---------+
| 2       |
| 3       |
| 6       |
+---------+ 
```

## Solution: 
```
SELECT DISTINCT a.user_id
FROM confirmations as a
JOIN confirmations as b
ON  a.user_id = b.user_id AND
    a.time_stamp > b.time_stamp AND
    timestampdiff(second, b.time_stamp,a.time_stamp) <=86400
```

`timestampdiff(unit,time_beg,time_end)`:
-  `unit`  can be FRAC_SECOND, SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, or YEAR.

# [1890\. Filter top X records ](https://leetcode.com/problems/the-latest-login-in-2020/submissions/)
```
Write an SQL query to report the **latest** login for all users in the year `2020`. Do **not** include the users who did not login in `2020`.

Return the result table **in any order**.

Input: 
Logins table:
+---------+---------------------+
| user_id | time_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 6       | 2021-04-21 14:06:06 |
| 6       | 2019-03-07 00:18:15 |
| 8       | 2020-02-01 05:10:53 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
| 2       | 2019-08-25 07:59:08 |
| 14      | 2019-07-14 09:00:00 |
| 14      | 2021-01-06 11:59:59 |
+---------+---------------------+
Output: 
+---------+---------------------+
| user_id | last_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
+---------+---------------------+
```

## Solution - Group by with max/min()
```mysql
SELECT user_id, max(time_stamp) as 'last_stamp'
FROM logins
WHERE year(time_stamp) = 2020 
GROUP BY user_id
```

# [1873\. CASE()](https://leetcode.com/problems/calculate-special-bonus/)

```
Write an SQL query to calculate the bonus of each employee. The bonus of an employee is `100%` of their salary if the ID of the employee is **an odd number** and **the employee name does not start with the character** `'M'`. The bonus of an employee is `0` otherwise.

Return the result table ordered by `employee_id`.

Employees table:
+-------------+---------+--------+
| employee_id | name    | salary |
+-------------+---------+--------+
| 2           | Meir    | 3000   |
| 3           | Michael | 3800   |
| 7           | Addilyn | 7400   |
| 8           | Juan    | 6100   |
| 9           | Kannon  | 7700   |
+-------------+---------+--------+
Output: 
+-------------+-------+
| employee_id | bonus |
+-------------+-------+
| 2           | 0     |
| 3           | 0     |
| 7           | 7400  |
| 8           | 0     |
| 9           | 7700  |
+-------------+-------+
```
## Solution
```mysql
SELECT employee_id,
(CASE
    WHEN employee_id%2 !=0 AND left(name,1) !='M' THEN salary
    ELSE 0
 END
)as bonus
FROM Employees
ORDER BY employee_id
```
# [1853\. Convert Date Format](https://leetcode.com/problems/convert-date-format/)
```
Write an SQL query to convert each date in `Days` into a string formatted as `"day_name, month_name day, year"`.

Return the result table in **any order**.

Input: 
Days table:
+------------+
| day        |
+------------+
| 2022-04-12 |
| 2021-08-09 |
| 2020-06-26 |
+------------+
Output: 
+-------------------------+
| day                     |
+-------------------------+
| Tuesday, April 12, 2022 |
| Monday, August 9, 2021  |
| Friday, June 26, 2020   |
+-------------------------+
Explanation: Please note that the output is case-sensitive.
```
## Solution
```mysql
SELECT
CONCAT(DAYNAME(day),", ",MONTHNAME(day)," ",day(day),", ",year(day)) as day
FROM days
 ```
- `CONCAT(a,b,c,...)` joins stings 
- `DAYNAME()` get name of the week
- `MONTHNAME()` get name of the month

# [196\. Remove duplicate via DELETE ](https://leetcode.com/problems/delete-duplicate-emails/submissions/)
```
Write an SQL query to **delete** all the duplicate emails, keeping only one unique email with the smallest `id`. 

Note that you are supposed to write a `DELETE` statement and not a `SELECT` one.
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Output: 
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
Explanation: john@example.com is repeated two times. We keep the row with the smallest Id = 1.
```

## Solution
```mysql
DELETE P2
FROM Person p1, Person P2
WHERE P1.email = p2.email and p1.id<p2.id
```