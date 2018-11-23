---
title: LeetCode 0064 - Minimum Path Sum
date: 2017-11-17 15:22:32
categories: LeetCode
---
# Minimum Path Sum #

<!--more-->

## Desicription ##

Given a *m x n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example 1:**

```
[[1,3,1],
 [1,5,1],
 [4,2,1]]
```

Given the above grid map, return `7`. Because the path 1→3→1→1→1 minimizes the sum.

## Solution ##

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        vector<int> dp(m, 0);
        dp[0] = grid[0][0];
        for(int i = 1; i < m; i++)
            dp[i] = grid[0][i] + dp[i-1];
        for(int i = 1; i < n; i++){
            dp[0] += grid[i][0];
            for(int j = 1; j < m; j++)
                dp[j] = min(dp[j], dp[j-1]) + grid[i][j];
        }
        return dp[m-1];
    }
};
```