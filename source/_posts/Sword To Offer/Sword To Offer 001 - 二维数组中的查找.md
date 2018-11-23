---
title: Sword To Offer 001 - 二维数组中的查找
date: 2018-03-11 22:57:14
categories: Sword To Offer
---
# 二维数组中的查找

<!--more-->

## Desicription

在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## Solution

```cpp
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        int row = array.size();
        if(row == 0) {
            return false;
        }
        int col = array[0].size();
        int currentRow = 0;
        int currentCol = col - 1;
        while(true) {
            if(currentRow >= row || currentCol < 0) {
                return false;
            }
            auto currentNumber = array[currentRow][currentCol];
            if(currentNumber == target) {
                return true;
            }
            else if(currentNumber > target) {
                currentCol--;
            }
            else {
                currentRow++;
            }
        }
    }
};
```