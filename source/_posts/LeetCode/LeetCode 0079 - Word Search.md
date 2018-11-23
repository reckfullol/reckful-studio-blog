---
title: LeetCode 0079 - Word Search
date: 2017-11-19 15:33:43
categories: LeetCode
---
# Word Search #

<!--more-->

## Desicription ##

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,
Given **board** =

```
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```

**word** = `"ABCCED"`, -> returns `true`,

**word** = `"SEE"`, -> returns `true`,

**word** = `"ABCB"`, -> returns `false`.

## Solution ##

```cpp
class Solution {
private:
    int board_row;
    int board_col;
    int dir[4][2] = { { 0, 1 },{ 0, -1 },{ 1, 0 },{ -1, 0 } };
    bool isFound(vector<vector<char>>& board, string word, int x, int y) {
        if (board[x][y] != word[0])
            return 0;
        else if (board[x][y] == word[0] && word.size() == 1)
            return 1;
        for (int i = 0; i < 4; ++i) {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if (dx < 0 || dx >= board_row || dy < 0 || dy >= board_col)
                continue;
            board[x][y] = '\0';
            if (isFound(board, word.substr(1), dx, dy))
                return 1;
            board[x][y] = word[0];
        }
        return 0;
    }

public:
    bool exist(vector<vector<char>>& board, string word) {
        board_row = board.size();
        board_col = board[0].size();
        for (int i = 0; i < board_row; ++i)
            for (int j = 0; j < board_col; ++j)
                if (isFound(board, word, i, j))
                    return 1;
        return 0;
    }
};
```