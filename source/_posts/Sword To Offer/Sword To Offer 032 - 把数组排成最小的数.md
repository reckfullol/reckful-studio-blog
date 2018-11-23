---
title: Sword To Offer 032 - 把数组排成最小的数
date: 2018-03-30 15:38:00
categories: Sword To Offer
---
# 把数组排成最小的数

<!--more-->

## Desicription

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

## Solution

```cpp
class Solution {
private :
    struct node {
        string init;
        string change;
    };
public:
    string PrintMinNumber(const vector<int>& numbers) {
        vector<node> tmp(0);
        for(auto a : numbers) {
            tmp.emplace_back(node{to_string(a), ""});
        }
        int maxLength = 0;
        for(const auto& a : tmp) {
            maxLength = max(maxLength, (int)a.init.length());
        }
        for(auto& a : tmp) {
            a.change = a.init + string(maxLength - a.init.length(), a.init[a.init.length() - 1]);
        }
        sort(tmp.begin(), tmp.end(), [](node a, node b) {return a.change < b.change;});
        string res;
        for(const auto &a : tmp) {
            res += a.init;
        }
        return res;
    }
};
```