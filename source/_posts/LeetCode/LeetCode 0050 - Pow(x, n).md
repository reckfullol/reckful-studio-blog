---
title: LeetCode 0050 - Pow(x, n)
date: 2017-11-12 21:16:46
categories: LeetCode
---
# Pow(x, n) #

<!--more-->

## Desicription ##

Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/).

**Example 1:**

```
Input: 2.00000, 10
Output: 1024.00000
```

**Example 2:**

```
Input: 2.10000, 3
Output: 9.26100
```

## Solution ##

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        return pow(x, n);
    }
};
```