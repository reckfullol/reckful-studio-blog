---
title: LeetCode 0289 - Game of Life
date: 2018-12-19 09:51:32
categories: LeetCode
---
# Game of Life

<!--more-->

## Desicription

According to the Wikipedia's article: "The **Game of Life**, also known simply as **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population..
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

**Example:**

```
Input: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

## Solution

```cpp
class Solution {
private:

public:
    void gameOfLife(vector<vector<int>>& board) {
        vector<vector<int>> dir {{1, 0}, {-1, 0}, {0, -1}, {0, 1}, {1, 1}, {1, -1}, {-1, 1}, {-1, -1}, {0, 0}};
        for(int i = 0; i < board.size(); i++) {
            for(int j = 0; j < board[0].size(); j++) {
                int cnt = 0;
                for(const auto& d : dir) {
                    int dx = i + d[0];
                    int dy = j + d[1];
                    if(dx < 0 || dx >= board.size() || dy < 0 || dy >= board[0].size()) {
                        continue;
                    }
                    cnt += board[dx][dy] & 1;
                }
                if(cnt == 3 || cnt - board[i][j] == 3) {
                    board[i][j] |= 2;
                }
            }
        }
        for(int i = 0; i < board.size(); i++) {
            for(int j = 0; j < board[0].size(); j++) {
                board[i][j] >>= 1;
            }
        }
    }
};
```