---
title: Sword To Offer 011 - 二进制中1的个数
date: 2018-03-15 17:37:48
categories: Sword To Offer
---
# 二进制中1的个数

<!--more-->

## Desicription

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

## Solution

```cpp
class Solution {
public:
    int NumberOf1(unsigned int n) {
        int count = 0;
        while(n) {
            count += n & 1;
            n >>= 1;
        }
        return count;
    }
};
```
