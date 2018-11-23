---
title: LeetCode 0085 - Maximal Rectangle
date: 2017-11-20 21:47:58
categories: LeetCode
---
# Maximal Rectangle #

<!--more-->

## Desicription ##

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

For example, given the following matrix:

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Return 6.

## Solution ##

```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        if(matrix.empty())
            return 0;
        int row_size = matrix.size();
        int col_size = matrix[0].size();
        vector<int> left(col_size, 0);
        vector<int> right(col_size, col_size);
        vector<int> height(col_size, 0);
        int res = 0;
        for(int i = 0; i < row_size; i++){
            int cur_left = 0;
            int cur_right = col_size;
            for(int j = 0; j < col_size; j++){
                if(matrix[i][j] == '1')
                    height[j]++;
                else
                    height[j] = 0;
            }
            for(int j = 0; j < col_size; j++){
                if(matrix[i][j] == '1')
                    left[j] = max(left[j], cur_left);
                else
                    left[j] = 0, cur_left = j + 1;
            }
            for(int j = col_size - 1; j >= 0; j--){
                if(matrix[i][j] == '1')
                    right[j] = min(right[j], cur_right);
                else
                    right[j] = col_size, cur_right = j;
            }
            for(int j = 0; j < col_size; j++)
                res = max(res, (right[j] - left[j]) * height[j]);
        }
        return res;
    }
};
```