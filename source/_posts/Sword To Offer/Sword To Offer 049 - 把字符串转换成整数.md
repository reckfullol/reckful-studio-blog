---
title: Sword To Offer 049 - 把字符串转换成整数
date: 2018-04-19 21:52:52
categories: Sword To Offer
---
# 把字符串转换成整数

<!--more-->

## Desicription

将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

## Solution

```cpp
class Solution {
public:
    int StrToInt(string str) {
        int index = 0;
        bool flag = true;
        if(str[0] == '+') {
            index = 1;
        } else if(str[0] == '-') {
            flag = false;
            index = 1;
        }
        int res = 0;
        for(; str[index]; index++) {
            if(!(str[index] >= '0' && str[index] <= '9')) {
                return 0;
            }
            res = res * 10 + str[index] - '0';
        }
        return flag ? res : -res;
    }
};
```