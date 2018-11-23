---
title: LeetCode 0059 - Spiral Matrix II
date: 2017-11-13 16:48:47
categories: LeetCode
---
# Spiral Matrix II #

<!--more-->

## Desicription ##

Given an integer *n*, generate a square matrix filled with elements from 1 to *n*2 in spiral order.

For example,

Given *n* = `3`,

You should return the following matrix:

```
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

## Solution ##

```cpp
class Solution {
private:
    bool isValid(vector<vector<int> >& matrix, int x, int y, int n){
        if(x < 0 || x >= n || y < 0 || y >= n || matrix[x][y] == INT_MIN)
            return 0;
        return 1;
    }
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> matrix(n, vector<int>(n, 0));
        vector<vector<int>> res(n, vector<int>(n, 0));
        int x = 0;
        int y = 0;
        int val = 1;
        while(1){
            bool flag = 0;
            while(isValid(matrix, x, y, n))
                res[x][y] = val++, matrix[x][y] = INT_MIN, y++, flag = 1;
            y--;
            x++;
            while(isValid(matrix, x, y, n))
                res[x][y] = val++, matrix[x][y] = INT_MIN, x++, flag = 1;
            x--;
            y--;
            while(isValid(matrix, x, y, n))
                res[x][y] = val++, matrix[x][y] = INT_MIN, y--, flag = 1;
            y++;
            x--;
            while(isValid(matrix, x, y, n))
                res[x][y] = val++, matrix[x][y] = INT_MIN, x--, flag = 1;
            x++;
            y++;
            if(!flag)
                return res;
        }
    }
};
```