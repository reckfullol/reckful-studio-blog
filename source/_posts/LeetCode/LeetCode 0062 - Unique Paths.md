---
title: LeetCode 0062 - Unique Paths
date: 2017-11-17 15:05:17
categories: LeetCode
---
# Unique Paths #

<!--more-->

## Desicription ##

A robot is located at the top-left corner of a *m x n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![fuck!](https://leetcode.com/static/images/problemset/robot_maze.png)

Above is a 3 x 7 grid. How many possible unique paths are there?

**Note:** *m* and *n* will be at most 100.

## Solution ##

```cpp
class Solution{
  public:
    int uniquePaths(int m, int n){
        int a = m - 1;
        int b = n + m - 2;
        double res = 1;
        for(int i = 1; i <= a; i++)
            res = res * (b - a + i) / i;
        return res;
    }
};
```