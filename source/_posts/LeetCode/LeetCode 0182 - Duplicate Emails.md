---
title: LeetCode 0182 - Duplicate Emails
date: 2018-06-04 21:02:14
categories: LeetCode
---
# Duplicate Emails

<!--more-->

## Desicription

Write a SQL query to find all duplicate emails in a table named `Person`.

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

For example, your query should return the following for the above table:

```
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

**Note**: All emails are in lowercase.

## Solution

```sql
SELECT DISTINCT P1.Email
AS Email
from Person P1, Person P2
WHERE P1.Email = P2.Email AND P1.Id != P2.Id
```