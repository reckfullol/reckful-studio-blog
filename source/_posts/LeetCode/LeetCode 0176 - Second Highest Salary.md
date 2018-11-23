---
title: LeetCode 0176 - Second Highest Salary
date: 2018-06-04 15:44:32
categories: LeetCode
---
# Second Highest Salary

<!--more-->

## Desicription


Write a SQL query to get the second highest salary from the `Employee` table.

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

For example, given the above Employee table, the query should return 200 as the second highest salary. If there is no second highest salary, then the query should return null.

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

## Solution

```sql
# Write your MySQL query statement below
select MAX(Salary)
as SecondHighestSalary
from Employee
where Salary < (select MAX(Salary) from Employee)
```