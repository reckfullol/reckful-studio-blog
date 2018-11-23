---
title: LeetCode 0172 - Factorial Trailing Zeroes
date: 2018-06-04 12:11:21
categories: LeetCode
---
# Factorial Trailing Zeroes

<!--more-->

## Desicription

Given an integer *n*, return the number of trailing zeroes in *n*!.

**Example** 1:

```
Input: 3
Output: 0
Explanation: 3! = 6, no trailing zero.
```

**Example** 2:

```
Input: 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```

**Note**: Your solution should be in logarithmic time complexity.

## Solution

```cpp
class Solution {
public:
    long long trailingZeroes(long long n) {
        long long res = 0;
        for(long long i = 5; n / i; i *= 5)
            res += n / i;
        return res;
    }
};
```

