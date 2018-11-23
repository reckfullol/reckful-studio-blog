---
title: Sword To Offer 009 - 变态跳台阶
date: 2018-03-15 17:20:38
categories: Sword To Offer
---
# 变态跳台阶

<!--more-->

## Desicription

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

## Solution

```cpp
class Solution {
private:
    int Pow(int number, int pow) {
        int res = 1;
        while(pow) {
            if(pow & 1) {
               res *= number;
            }
            number *= number;
            pow >>= 1;
        }
        return res;
    }
public:
    int jumpFloorII(int number) {
        return Pow(2, number - 1);
    }
};
```