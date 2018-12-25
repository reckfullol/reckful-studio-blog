---
title: LeetCode 0304 - Range Sum Query 2D - Immutable
date: 2018-12-25 15:36:56
categories: LeetCode
---
# Range Sum Query 2D - Immutable

<!--more-->

## Desicription

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

![fuck](https://leetcode.com/static/images/courses/range_sum_query_2d.png)

Range Sum Query 2D
The above rectangle (with the red border) is defined by (row1, col1) = (2, 1) and (row2, col2) = (4, 3), which contains sum = 8.

**Example:**

```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

**Note:**

1. You may assume that the matrix does not change.
2. There are many calls to sumRegion function.
3. You may assume that row1 ≤ row2 and col1 ≤ col2.

## Solution

```cpp
class NumMatrix {
    vector<vector<int>> dp;
public:
    explicit NumMatrix(const vector<vector<int>> &matrix) {
        if(matrix.empty()) {
            return ;
        }
        dp = vector<vector<int>>(matrix.size(), vector<int>(matrix[0].size()));
        unsigned long row = matrix.size();
        unsigned long col = matrix[0].size();
        dp[0][0] = matrix[0][0];
        for(int i = 1; i < row; i++) {
            dp[i][0] = dp[i - 1][0] + matrix[i][0];
        }
        for(int i = 1; i < col; i++) {
            dp[0][i] = dp[0][i - 1] + matrix[0][i];
        }
        for(int i = 1; i < row; i++) {
            for(int j = 1; j < col; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1] - dp[i - 1][j - 1] + matrix[i][j];
            }
        }
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        if(dp.empty()) {
            return 0;
        }
        if(row1 == 0 && col1 == 0) {
            return dp[row2][col2];
        } else if(row1 == 0) {
            return dp[row2][col2] - dp[row2][col1 - 1];
        } else if(col1 == 0) {
            return dp[row2][col2] - dp[row1 - 1][col2];
        } else {
            return dp[row2][col2] - dp[row2][col1 - 1] - dp[row1 - 1][col2] + dp[row1 - 1][col1 - 1];
        }
    }
};
```