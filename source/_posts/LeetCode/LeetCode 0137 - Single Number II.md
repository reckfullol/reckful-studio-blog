---
title: LeetCode 0137 - Single Number II
date: 2018-05-28 14:43:29
categories: LeetCode
---
# Single Number II

<!--more-->

## Desicription

Given a **non-empty** array of integers, every element appears *three* times except for one, which appears exactly once. Find that single one.

**Note**:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example** 1:

```
Input: [2,2,3,2]
Output: 3
```

**Example** 2:

```
Input: [0,1,0,1,0,1,99]
Output: 99
```

## Solution

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int a = 0;
        int b = 0;
        for(auto it : nums) {
            a = (a ^ it) & ~b;
            b = (b ^ it) & ~a;
        }
        return a;
    }
};
```

