---
title: LeetCode 0036 - Valid Sudoku
date: 2017-11-11 15:21:28
categories: LeetCode
---
# Valid Sudoku #

<!--more-->

## Desicription ##

Determine if a Sudoku is valid, according to: [Sudoku Puzzles - The Rules](http://sudoku.com.au/TheRules.aspx).

The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.

![fuck](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

*A partially filled sudoku which is valid.*

**Note:**

A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.

## Solution ##

```cpp
class Solution {
private:
    bool book1[9][9] = {0};
    bool book2[9][9] = {0};
    bool book3[9][9] = {0};
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        for(int i = 0; i < 9; i++){
            for(int j = 0 ; j < 9; j++){
                if(board[i][j] != '.'){
                    int num = board[i][j] - '0' - 1;
                    int key = i / 3 * 3 + j / 3;
                    if(book1[i][num] || book2[j][num] || book3[key][num])
                        return 0;
                    book1[i][num] = book2[j][num] = book3[key][num] =1;
                }
            }
        }
        return 1;   
    }
};  

```