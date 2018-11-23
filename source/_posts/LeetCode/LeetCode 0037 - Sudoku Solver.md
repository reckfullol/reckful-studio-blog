---
title: LeetCode 0037 - Sudoku Solver
date: 2017-11-11 15:26:17
categories: LeetCode
---
# Sudoku Solver #

<!--more-->

## Desicription ##

Write a program to solve a Sudoku puzzle by filling the empty cells.

Empty cells are indicated by the character `'.'`.

You may assume that there will be only one unique solution.

![fuck](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

*A sudoku puzzle...*

![fuck](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

*...and its solution numbers marked in red.*

## Solution ##

```cpp
class Solution {
private:
    bool row[10][10] = {0};
    bool col[10][10] = {0};
    bool block[10][10] = {0};
    bool flag = 0;
public:
    void solveSudoku(vector<vector<char>>& board) {
        for(int i = 0; i < 9; i++){
            for(int j = 0; j < 9; j++){
                if(board[i][j] == '.')
                    continue;
                int index = i / 3 * 3 + j / 3;
                int num = board[i][j] - '0';
                row[i][num] = 1;
                col[j][num] = 1;
                block[index][num] = 1;
            }
        }
        dfs(board, 0, 0);
    }
    void dfs(vector<vector<char>>& board, int x, int y){
        if(flag)
            return ;
        if(x == 9){
            flag = 1;
            return ;
        }
        if(board[x][y] != '.'){
            if(y < 8)
                dfs(board, x, y+1);
            else
                dfs(board, x+1, 0);
            return ;
        }
        int index = x / 3 * 3 + y / 3;
        for(int num = 1; num <= 9; num++){
            if(row[x][num] || col[y][num] || block[index][num])
                continue;
            row[x][num] = col[y][num] = block[index][num] = 1;
            board[x][y] = num + '0';
            if(y < 8)
                dfs(board, x, y+1);
            else
                dfs(board, x+1, 0);
            if(flag)
                return ;
            row[x][num] = col[y][num] = block[index][num] = 0;
            board[x][y] = '.';
        } 
    }
};
```