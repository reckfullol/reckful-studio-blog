---
title: LeetCode 0342 - Power of Four
date: 2019-06-05 02:12:08
categories: LeetCode
---
# Power of Four

<!--more-->

## Desicription

Given an integer (signed 32 bits), write a function to check whether it is a power of 4.

**Example 1:**

```
Input: 16
Output: true
```

**Example 2:**

```
Input: 5
Output: false
```

Follow up: Could you solve it without loops/recursion?

## Solution

```cpp
class Solution {
public:
    bool isPowerOfFour(int num) {
        return num > 0 && ((num & (num - 1)) == 0) && (num & 0x55555555) != 0;
    }
};
```