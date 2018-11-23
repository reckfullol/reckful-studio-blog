---
title: LeetCode 0063 - Unique Paths II
date: 2017-11-17 15:08:55
categories: LeetCode
---
# Unique Paths II #

<!--more-->

## Desicription ##

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as `1` and `0` respectively in the grid.

For example,
There is one obstacle in the middle of a 3x3 grid as illustrated below.

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```

The total number of unique paths is `2`.

**Note:** *m* and *n* will be at most 100.

## Solution ##

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int n = obstacleGrid.size();
        int m = obstacleGrid[0].size();
        vector<vector<int>> dp(n+1, vector<int>(m+1, 0));
        dp[0][1] = 1;
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                if(!obstacleGrid[i-1][j-1]){
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        return dp[n][m];
    }
};
```