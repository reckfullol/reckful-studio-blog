---
title: Sword To Offer 043 - 左旋转字符串
date: 2018-04-17 18:30:21
categories: Sword To Offer
---
# 左旋转字符串

<!--more-->

## Desicription

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

## Solution

```cpp
class Solution {
public:
    string LeftRotateString(const string& str, int n) {
        if(str.empty()) {
            return str;
        }
        n %= str.length();
        return str.substr(static_cast<unsigned int>(n)) + str.substr(0, static_cast<unsigned int>(n));
    }
};
```