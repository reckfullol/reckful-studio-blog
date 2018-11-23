---
title: Sword To Offer 037 - 数字在排序数组中出现的次数
date: 2018-03-31 17:23:55
categories: Sword To Offer
---
# 数字在排序数组中出现的次数

<!--more-->

## Desicription

统计一个数字在排序数组中出现的次数。

## Solution

```cpp
class Solution {
public:
    int GetNumberOfK(const vector<int>& data ,int k) {
        return upper_bound(data.begin(), data.end(), k) - lower_bound(data.begin(), data.end(), k);
    }
};
```