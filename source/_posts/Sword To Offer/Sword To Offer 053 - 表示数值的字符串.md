---
title: Sword To Offer 053 - 表示数值的字符串
date: 2018-05-12 16:33:56
categories: Sword To Offer
---
# 表示数值的字符串

<!--more-->

## Desicription

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

## Solution

```cpp
class Solution {
public:
    bool isNumeric(string s) {
        int index = 0;
        for(; s[index] == ' '; index++);
        if(s[index] == '+' || s[index] == '-')
            index++;
        int n_num = 0, n_pt = 0;
        for(; ((s[index] <= '9' && s[index] >= '0') || s[index] == '.'); index++)
            s[index] == '.'?n_pt++:n_num++;
        if(!n_num || n_pt > 1)
            return false;
        if(s[index] == 'e' || s[index] == 'E'){
            index++;
            if(s[index] == '+' || s[index] == '-')
                index++;
            int nNum = 0;
            for(; s[index] >= '0' && s[index] <= '9'; index++)
                nNum++;
            if(!nNum)
                return false;
        }
        for(; s[index] == ' '; index++);
        return !s[index];
    }
};
```