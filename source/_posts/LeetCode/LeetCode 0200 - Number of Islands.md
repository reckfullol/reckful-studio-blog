---
title: LeetCode 0200 - Number of Islands
date: 2018-06-12 11:47:47
categories: LeetCode
---
# Number of Islands

<!--more-->

## Desicription

Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example** 1:

```
Input:
11110
11010
11000
00000

Output: 1
```

**Example** 2:

```
Input:
11000
11000
00100
00011

Output: 3
```

## Solution

```cpp
class Solution {
private:
    int n;
    int m;
    int dir[4][2] = {1, 0, -1, 0, 0, -1, 0, 1};
    void dfs(int x, int y, vector<vector<char>>& grid) {
        if(grid[x][y] == '1')
            grid[x][y] = '0';
        for(int i = 0; i < 4; i++) {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if(dx < 0 || dx == n || dy < 0 || dy >= m || grid[dx][dy] == '0')
                continue;
            dfs(dx, dy, grid);
        }
    }
public:
    int numIslands(vector<vector<char>>& grid) {
        if(grid.size() == 0)
            return 0;
        n = grid.size();
        m = grid[0].size();
        int res = 0;
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(grid[i][j] == '1') {
                    res++;
                    dfs(i, j, grid);
                }
            }
        }
        return res;
    }
};
```