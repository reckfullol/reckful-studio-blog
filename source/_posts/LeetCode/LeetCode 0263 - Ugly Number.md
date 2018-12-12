---
title: LeetCode 0263 - Ugly Number
date: 2018-12-12 09:43:54
categories: LeetCode
---
# Ugly Number

<!--more-->

## Desicription

Write a program to check whether a given number is an ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`.

**Example 1:**

```
Input: 6
Output: true
Explanation: 6 = 2 × 3
```

**Example 2:**

```
Input: 8
Output: true
Explanation: 8 = 2 × 2 × 2
```
**Example 3:**

```
Input: 14
Output: false 
Explanation: 14 is not ugly since it includes another prime factor 7.
```

**Note:**

1. 1 is typically treated as an ugly number.
2. Input is within the 32-bit signed integer range: [−2^31,  2^31 − 1].

## Solution

```cpp
class Solution {
private:
public:
    bool isUgly(int num) {
        if(!num) {
            return false;
        }
        while(!(num % 2)) {
            num /= 2;
        }
        while(!(num % 3)) {
            num /= 3;
        }
        while(!(num % 5)) {
            num /= 5;
        }
        return num == 1;
    }
};
```