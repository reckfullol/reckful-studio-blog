---
title: LeetCode 0007 - Reverse Integer
date: 2017-11-08 08:57:18
categories: LeetCode
---
# Reverse Integer #

<!--more-->

## Desicription ##

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```
Input: 123
Output:  321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

**Note:**

Assume we are dealing with an environment which could only hold integers within the 32-bit signed integer range. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## Solution ##

```cpp
class Solution {
public:
    int reverse(int x) {
        long long res = 0;
        while(x){
            res *= 10;
            res += x % 10;
            x /= 10;
        }
        if(res < INT_MIN || res > INT_MAX)
            return 0;
        return res;
    }
};
```