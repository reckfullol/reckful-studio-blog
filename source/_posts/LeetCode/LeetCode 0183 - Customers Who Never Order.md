---
title: LeetCode 0183 - Customers Who Never Order
date: 2018-06-04 21:17:22
categories: LeetCode
---
# Customers Who Never Order

<!--more-->

## Desicription

Suppose that a website contains two tables, the `Customers` table and the `Orders` table. Write a SQL query to find all customers who never order anything.

Table: `Customers`.

```
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
```

Table: `Orders`.

```
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

Using the above tables as example, return the following:

```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

## Solution

```sql
SELECT Name
AS Customers
FROM Customers
WHERE Id NOT IN
(SELECT CustomerId FROM Orders)
```