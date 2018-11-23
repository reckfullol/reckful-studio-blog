---
title: LeetCode 0051 - N-Queens
date: 2017-11-13 13:20:11
categories: LeetCode
---
# N-Queens #

<!--more-->

## Desicription ##

The *n*-queens puzzle is the problem of placing *n* queens on an n×n chessboard such that no two queens attack each other.

![fuck](https://leetcode.com/static/images/problemset/8-queens.png)

Given an integer *n*, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space respectively.

For example,
There exist two distinct solutions to the 4-queens puzzle:

```
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

## Solution ##

```cpp
class Solution {
private:
    vector<vector<string>> res;
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<int> state(n, -1);
        dfs(state, 0);
        return res;
    }
    void dfs(vector<int>& state, int row){
        int n = state.size();
        if(row == n){
            vector<string> tmp(n, string(n, '.'));
            for(int i = 0; i < n; i++)
                tmp[i][state[i]] = 'Q';
            res.push_back(tmp);
            return ;
        }
        for(int col = 0; col < n; col++){
            if(isVaild(state, row, col)){
                state[row] = col;
                dfs(state, row+1);
                state[row] = -1;
            }
        }
    }
    bool isVaild(vector<int>& state, int row, int col){
        for(int i = 0; i < row; i++){
            if(state[i] == col || abs(row - i) == abs(col - state[i]))
                return 0;
        }
        return 1;
    }
};
```