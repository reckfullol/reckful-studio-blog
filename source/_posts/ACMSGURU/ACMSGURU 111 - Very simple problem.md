---
title: ACMSGURU 111 - Very simple problem
date: 2019-12-18 10:44:15
categories: ACMSGURU
---
# Very simple problem

<!--more-->

## Problem Description

You are given natural number X. Find such maximum integer number that it square is not greater than X.

## Input

Input file contains number X (1≤X≤10^1000).

## Output

Write answer in output file.

## Sample Input

16

## Sample Output

4

## Solution

```python
if __name__ == '__main__':

    n = int(input())
    x = n
    y = (x + 1) // 2
    while y < x:
        x = y
        y = (x + n // x) // 2

    print(x)
```

