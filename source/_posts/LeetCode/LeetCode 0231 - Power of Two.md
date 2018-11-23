---
title: LeetCode 0231 - Power of Two
date: 2018-08-17 08:09:05
categories: LeetCode
---
# Power of Two

<!--more-->

## Desicription

Given an integer, write a function to determine if it is a power of two.

**Example 1:**

```
Input: 1
Output: true 
Explanation: 20 = 1
```

**Example 2:**

```
Input: 16
Output: true
Explanation: 24 = 16
```

**Example 3:**

```
Input: 218
Output: false
```

## Solution

```cpp
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if(n <= 0) {
            return false;
        } else {
            return !(n&(n-1));
        }
    }
};
```