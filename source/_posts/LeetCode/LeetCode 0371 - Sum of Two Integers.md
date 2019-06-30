---
title: LeetCode 0371 - Sum of Two Integers
date: 2019-06-30 17:24:11
categories: LeetCode
---
# Sum of Two Integers

<!--more-->

## Desicription

Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

**Example 1:**

```
Input: a = 1, b = 2
Output: 3
```

**Example 2:**

```
Input: a = -2, b = 3
Output: 1
```

## Solution

```cpp
class Solution {
public:
    int getSum(int a, int b) {
        __asm__(
                "addl %%ebx, %%eax;"
                : "=a"(a)
                : "a"(a), "b"(b)
                );
        return a;
    }
};
```