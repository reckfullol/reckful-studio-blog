---
title: LeetCode 0029 - Divide Two Integers
date: 2017-11-10 19:41:54
categories: LeetCode
---
# Divide Two Integers #

<!--more-->

## Desicription ##

Divide two integers without using multiplication, division and mod operator.

If it is overflow, return MAX_INT.

## Solution ##

```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        if(!divisor || (dividend == INT_MIN && divisor == -1))
            return INT_MAX;
        int flag = ((dividend < 0) ^ (divisor < 0))?-1:1;
        long long m = abs((long long)dividend);
        long long n = abs((long long)divisor);
        int res = 0;
        while(m >= n){
            long long t = n;
            long long p = 1;
            while((t << 1) <= m){
                t <<= 1;
                p <<= 1;
            }
            m -= t;
            res += p;
        }
        return flag*res;
    }
};
```