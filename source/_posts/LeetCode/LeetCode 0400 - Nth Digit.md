---
title: LeetCode 0400 - Nth Digit
date: 2019-10-02 16:27:56
categories: LeetCode
---
# LeetCode 0400 - Nth Digit

<!--more-->

## Desicription

Find the nth digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...

**Note:**

n is positive and will fit within the range of a 32-bit signed integer (n < 231).

**Example 1:**

```
Input:
3

Output:
3
```

**Example 2:**

```
Input:
11

Output:
0

Explanation:
The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.
```

## Solution

```cpp
class Solution {
public:
    int findNthDigit(int n) {
        long long start = 1;
        long long len = 1;
        long long count = 9;
        while(n > len * count) {
            n -= len * count;
            len++;
            count *= 10;
            start *= 10;
        }
        return std::to_string(start + (n - 1) / len)[(n - 1) % len] - '0';
    }
};
```
