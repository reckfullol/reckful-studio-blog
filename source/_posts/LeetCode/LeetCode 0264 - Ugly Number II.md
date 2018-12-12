---
title: LeetCode 0264 - Ugly Number II
date: 2018-12-12 09:45:43
categories: LeetCode
---
# Ugly Number II

<!--more-->

## Desicription

Write a program to find the n-th ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`. 

**Example:**

```
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
```

**Note:**  

1. 1 is typically treated as an ugly number.
2. n does not exceed 1690.

## Solution

```cpp
class Solution {
public:
    long long nthUglyNumber(int n) {
        set<long long> ugly_number;
        ugly_number.insert(1);
        while(n--) {
            if(!n) {
                return *ugly_number.begin();
            }
            const auto& top = ugly_number.begin();
            ugly_number.insert(*top * 2);
            ugly_number.insert(*top * 3);
            ugly_number.insert(*top * 5);
            ugly_number.erase(top);
        }
    }
};
```