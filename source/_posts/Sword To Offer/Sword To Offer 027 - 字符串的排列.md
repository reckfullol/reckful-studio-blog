---
title: Sword To Offer 027 - 字符串的排列
date: 2018-03-27 14:55:24
categories: Sword To Offer
---
# 字符串的排列

<!--more-->

## Desicription

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

## Solution

```cpp
class Solution {
public:
    vector<string> Permutation(string str) {
        vector<string> res;
        if(str == "")
            return res;
        do {
            res.push_back(str);
        } while(next_permutation(str.begin(), str.end()));
        return res;
    }
};
```