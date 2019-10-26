---
title: ACMSGURU 112 - a^b-b^a
date: 2019-10-26 11:25:37
categories: ACMSGURU
---
# a^b-b^a

<!--more-->

## Problem Description

You are given natural numbers a and b. Find a^b-b^a.

## Input

Input contains numbers a and b (1≤a,b≤100).

## Output

Write answer to output.

## Sample Input

2 3

## Sample Output

-1

## Solution

```python
if __name__ == '__main__':

    x, y = map(int, input().split())

    def quick_pow(a, n):
        res = 1
        while n != 0:
            if n & 1:
                res *= a
            a *= a
            n >>= 1
        return res

    print(quick_pow(x, y) - quick_pow(y, x))
```