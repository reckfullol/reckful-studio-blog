---
title: LeetCode 0073 - Set Matrix Zeroes
date: 2017-11-19 13:35:44
categories: LeetCode
---
# Set Matrix Zeroes #

<!--more-->

## Desicription ##

Given a *m x n* matrix, if an element is 0, set its entire row and column to 0. Do it in place.

## Solution ##

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int n = matrix.size();
        int m = matrix[0].size();
        vector<bool> row(n, 0);
        vector<bool> col(m, 0);
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < m; ++j)
                if(!matrix[i][j])
                    row[i] = 1, col[j] = 1;
        for(int i = 0; i < n; ++i){
            if(row[i]){
                for(int j = 0; j < m; ++j)
                    matrix[i][j] = 0;
            }
        }
        for(int j = 0; j < m; ++j){
            if(col[j]){
                for(int i = 0; i < n; ++i)
                    matrix[i][j] = 0;
            }
        }
    }
};
```