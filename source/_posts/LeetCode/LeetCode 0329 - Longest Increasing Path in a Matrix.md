---
title: LeetCode 0329 - Longest Increasing Path in a Matrix
date: 2019-05-29 14:14:34
categories: LeetCode
---
# Longest Increasing Path in a Matrix

<!--more-->

## Desicription

Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).

**Example 1:**

```
Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].
```

**Example 2:**

```
Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

## Solution

```cpp
class Solution {
public:
    int longestIncreasingPath(const std::vector<std::vector<int>>& matrix) {
        if(matrix.empty()) {
            return 0;
        }

        int row = matrix.size();
        int col = matrix[0].size();
        auto dp = std::vector<std::vector<int>>(row, std::vector<int>(col, 0));

        std::function<int(int, int)> dfs = [&](int x, int y) {
            if(dp[x][y] != 0) {
                return dp[x][y];
            }
            auto dirs = std::vector<std::vector<int>>{{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
            for(auto& dir : dirs) {
                int dx = x + dir[0];
                int dy = y + dir[1];

                if(dx >= 0 && dx < row && dy >= 0 && dy < col && matrix[dx][dy] > matrix[x][y]) {
                    dp[x][y] = std::max(dp[x][y], dfs(dx, dy));
                }
            }
            return ++dp[x][y];
        };

        int res = 0;
        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                res = std::max(res, dfs(i, j));
            }
        }

        return res;
    }
};
```