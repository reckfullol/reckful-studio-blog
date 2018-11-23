---
title: Sword To Offer 013 - 调整数组顺序使奇数位于偶数前面
date: 2018-03-16 10:34:19
categories: Sword To Offer
---
# 调整数组顺序使奇数位于偶数前面

<!--more-->

## Desicription

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

## Solution

```cpp
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        stable_partition(array.begin(), array.end(), [](int n){return n & 1;});
    }
};
```