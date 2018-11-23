---
title: Sword To Offer 042 - 和为S的两个数字
date: 2018-04-18 21:23:37
categories: Sword To Offer
---
# 和为S的两个数字

<!--more-->

## Desicription

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

## Solution

```cpp
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        for(int i = 0; i < array.size(); i++) {
            auto ret = lower_bound(array.begin() + i + 1, array.end(), sum - array[i]);
            if(ret != array.end() && array[i] + *ret == sum) {
                return vector<int>{array[i], *ret};
            }
        }
        return vector<int>();
    }
};
```