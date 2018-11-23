---
title: LeetCode 0201 - Bitwise AND of Numbers Range
date: 2018-06-12 11:49:26
categories: LeetCode
---
# Bitwise AND of Numbers Range

<!--more-->

## Desicription

Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.

**Example** 1:

```
Input: [5,7]
Output: 4
```

**Example** 2:

```
Input: [0,1]
Output: 0
```

## Solution

```cpp
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        return n>m?(rangeBitwiseAnd(m>>1, n>>1) << 1):n;
    }
};
```