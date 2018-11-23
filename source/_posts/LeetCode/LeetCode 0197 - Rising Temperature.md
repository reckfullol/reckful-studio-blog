---
title: LeetCode 0197 - Rising Temperature
date: 2018-06-11 14:23:42
categories: LeetCode
---
# Rising Temperature

<!--more-->

## Desicription

Given a `Weather` table, write a SQL query to find all dates' Ids with higher temperature compared to its previous (yesterday's) dates.

```
+---------+------------------+------------------+
| Id(INT) | RecordDate(DATE) | Temperature(INT) |
+---------+------------------+------------------+
|       1 |       2015-01-01 |               10 |
|       2 |       2015-01-02 |               25 |
|       3 |       2015-01-03 |               20 |
|       4 |       2015-01-04 |               30 |
+---------+------------------+------------------+
```

For example, return the following Ids for the above `Weather` table:

```
+----+
| Id |
+----+
|  2 |
|  4 |
+----+
```

## Solution

```sql
# Write your MySQL query statement below
SELECT 
    W1.Id
FROM
    Weather W1, Weather W2
WHERE
    W1.Temperature > W2.Temperature AND
    TO_DAYS(W1.Date) - TO_DAYS(W2.Date) = 1
```