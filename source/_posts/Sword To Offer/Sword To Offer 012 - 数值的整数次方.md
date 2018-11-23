---
title: Sword To Offer 012 - 数值的整数次方
date: 2018-03-15 17:54:44
categories: Sword To Offer
---
# 数值的整数次方

<!--more-->

## Desicription

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

## Solution

```cpp
class Solution {
private:
    double QuickPower(double base, int exponent) {
        double res = 1.0;
        if(exponent < 0) {
            base = 1.0 / base;
            exponent = -exponent;
        }
        while(exponent) {
            if(exponent & 1) {
                res *= base;
            }
            base *= base;
            exponent >>= 1;
        }
        return res;
    }
public:
    double Power(double base, int exponent) {
        return QuickPower(base, exponent);
    }
};
```