---
title: LeetCode 0054 - Spiral Matrix
date: 2017-11-13 16:01:15
categories: LeetCode
---
# Spiral Matrix #

<!--more-->

## Desicription ##

Given a matrix of *m x n* elements (*m* rows, *n* columns), return all elements of the matrix in spiral order.

For example,
Given the following matrix:

```
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
```

You should return `[1,2,3,6,9,8,7,4,5]`.

## Solution ##

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
    vector<int> spiralOrder(vector<vector<int> >& matrix) {
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