---
title: LeetCode 0184 - Department Highest Salary
date: 2018-06-04 22:05:22
categories: LeetCode
---
# Department Highest Salary

<!--more-->

## Desicription

The `Employee` table holds all employees. Every employee has an Id, a salary, and there is also a column for the department Id.

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```

The `Department` table holds all departments of the company.

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

Write a SQL query to find employees who have the highest salary in each of the departments. For the above tables, Max has the highest salary in the IT department and Henry has the highest salary in the Sales department.

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

## Solution

```sql
SELECT 
    D.Name AS Department,
    E.Name AS Employee,
    E.Salary AS Salary
FROM
    Department D,
    Employee E,
    (SELECT DepartmentId AS Id, max(Salary) AS Salary FROM Employee GROUP BY DepartmentId) T
WHERE
    E.DepartmentId = D.Id AND
    E.DepartmentId = T.Id AND
    E.Salary = T.Salary
```