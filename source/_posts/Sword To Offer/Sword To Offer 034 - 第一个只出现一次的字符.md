---
title: Sword To Offer 034 - 第一个只出现一次的字符
date: 2018-03-30 19:50:22
categories: Sword To Offer
---
# 第一个只出现一次的字符

<!--more-->

## Desicription

在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置

## Solution

```cpp
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        vector<int> book(256, 0);
        for(int i = 0; str[i]; i++) {
            book[str[i]]++;
        }
        for(int i = 0; str[i]; i++) {
            if(book[str[i]] == 1) {
                return i;
            }
        }
        return -1;
    }
};
```