---
title: LeetCode 0052 - N-Queens II
date: 2017-11-13 15:34:24
categories: LeetCode
---
# N-Queens II #

<!--more-->

## Desicription ##

Follow up for N-Queens problem.

Now, instead outputting board configurations, return the total number of distinct solutions.

![fuck](https://leetcode.com/static/images/problemset/8-queens.png)

## Solution ##

```cpp
class Solution {
private:
    int res = 0;
public:
    int totalNQueens(int n) {
        vector<int> state(n, -1);
        dfs(state, 0);
        return res;
    }
    void dfs(vector<int>& state, int row){
        int n = state.size();
        if(row == n){
            res++;
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