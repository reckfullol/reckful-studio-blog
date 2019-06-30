---
title: LeetCode 0367 - Valid Perfect Square
date: 2019-06-30 15:01:13
categories: LeetCode
---
# Valid Perfect Square

<!--more-->

## Desicription

Given a positive integer num, write a function which returns True if num is a perfect square else False.

Note: Do not use any built-in library function such as sqrt.

**Example 1:**

```
Input: 16
Output: true
```

**Example 2:**

```
Input: 14
Output: false
```

## Solution

```cpp
class Solution {
public:
    bool isPerfectSquare(int num) {
        return int(pow(int(sqrt(num)), 2.0)) == num;
    }
};
```