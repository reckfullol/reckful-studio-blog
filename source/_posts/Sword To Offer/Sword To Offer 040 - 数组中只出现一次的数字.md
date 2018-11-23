---
title: Sword To Offer 040 - 数组中只出现一次的数字
date: 2018-03-31 21:21:44
categories: Sword To Offer
---
# 数组中只出现一次的数字

<!--more-->

## Desicription

一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

## Solution

```cpp
class Solution {
public:
    void FindNumsAppearOnce(const vector<int>& data, int* num1, int* num2) {
        int andNumber = 0;
        for(auto number : data) {
            andNumber ^= number;
        }
        int index = 0;
        while(andNumber ^ 1) {
            andNumber >>= 1;
            index++;
        }
        vector<int> partA;
        vector<int> partB;
        for(auto number : data) {
            if((number >> index) & 1) {
                partA.emplace_back(number);
            } else {
                partB.emplace_back(number);
            }
        }
        andNumber = 0;
        for(auto number : partA) {
            andNumber ^= number;
        }
        *num1 = andNumber;
        andNumber = 0;
        for(auto number : partB) {
            andNumber ^= number;
        }
        *num2 = andNumber;
    }
};
```