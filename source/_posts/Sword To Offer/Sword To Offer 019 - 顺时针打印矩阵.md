---
title: Sword To Offer 019 - 顺时针打印矩阵
date: 2018-03-16 17:52:37
categories: Sword To Offer
---
# 顺时针打印矩阵

<!--more-->

## Desicription

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字.

## Solution

```cpp
class Solution {
private:
    int n, m;
    bool isValid(vector<vector<int> >& matrix, int x, int y){
        if(x < 0 || x >= n || y < 0 || y >= m || matrix[x][y] == INT_MIN)
            return 0;
        return 1;
    }
public:
    vector<int> printMatrix(vector<vector<int> >& matrix) {
        vector<int> res;
        if(matrix.empty())
            return res;
        n = matrix.size();
        m = matrix[0].size();
        int x = 0;
        int y = 0;
        while(1){
            bool flag = 0;
            while(isValid(matrix, x, y))
                res.push_back(matrix[x][y]), matrix[x][y] = INT_MIN, y++, flag = 1;
            y--;
            x++;
            while(isValid(matrix, x, y))
                res.push_back(matrix[x][y]), matrix[x][y] = INT_MIN, x++, flag = 1;
            x--;
            y--;
            while(isValid(matrix, x, y))
                res.push_back(matrix[x][y]), matrix[x][y] = INT_MIN, y--, flag = 1;
            y++;
            x--;
            while(isValid(matrix, x, y))
                res.push_back(matrix[x][y]), matrix[x][y] = INT_MIN, x--, flag = 1;
            x++;
            y++;
            if(!flag)
                return res;
        }
    }
};
```