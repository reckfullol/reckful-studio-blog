---
title: Sword To Offer 048 - 不用加减乘除做加法
date: 2018-04-19 11:27:09
categories: Sword To Offer
---
# 不用加减乘除做加法

<!--more-->

## Desicription

写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

## Solution

```cpp
class Solution {
public:
    int Add(int num1, int num2) {
//        while(num2 != 0) {
//            int temp = num1 ^ num2;
//            num2 = (num1 & num2) << 1;
//            num1 = temp;
//        }
//        return num1;
        __asm__(    "addl %%ebx, %%eax;"
                    : "=a"(num1)
                    : "a" (num1), "b" (num2)
        );
        return num1;
    }
};
```