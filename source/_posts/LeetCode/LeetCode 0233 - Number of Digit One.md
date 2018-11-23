---
title: LeetCode 0233 - Number of Digit One
date: 2018-09-02 22:32:14
categories: LeetCode
---
# Number of Digit One

<!--more-->

## Desicription

Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

**Example:**

```
Input: 13
Output: 6 
Explanation: Digit 1 occurred in the following numbers: 1, 10, 11, 12, 13.
```

## Solution

```cpp
class Solution {
public:
    int countDigitOne(int n) {
        int count = 0;
        for(long long m = 1; m <= n; m*= 10) {
            count += (n / m + 8) / 10 * m + (n / m % 10 == 1) * (n % m + 1);
        }
        return count;
    }
};
```