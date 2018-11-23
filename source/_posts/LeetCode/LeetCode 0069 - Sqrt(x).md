---
title: LeetCode 0069 - Sqrt(x)
date: 2017-11-18 20:44:59
categories: LeetCode
---
# Sqrt(x) #

<!--more-->

## Desicription ##

Implement `int sqrt(int x)`.

Compute and return the square root of *x*.

**x** is guaranteed to be a non-negative integer.


**Example 1:**

```
Input: 4
Output: 2
```

**Example 2:**

```
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we want to return an integer, the decimal part will be truncated.
```

## Solution ##

```cpp
class Solution {
public:
    int mySqrt(int x) {
        long r = x;
        while (r*r > x)
            r = (r + x/r) / 2;
        return r;
    }
};
```