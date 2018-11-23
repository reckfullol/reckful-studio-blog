---
title: LeetCode 0180 - Consecutive Numbers
date: 2018-06-04 20:36:17
categories: LeetCode
---
# Consecutive Numbers

<!--more-->

## Desicription

Write a SQL query to find all numbers that appear at least three times consecutively.

```
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```

For example, given the above `Logs` table, `1` is the only number that appears consecutively for at least three times.

```
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

## Solution

```sql
SELECT DISTINCT L1.Num
AS ConsecutiveNums
from Logs L1, Logs L2, Logs L3
WHERE L1.Num = l2.Num AND L1.Num = L3.num AND L1.Id = L2.Id-1 AND L1.Id = L3.Id+1
```