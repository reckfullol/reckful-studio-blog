---
title: LeetCode 0130 - Surrounded Regions
date: 2018-05-24 16:03:23
categories: LeetCode
---
# Surrounded Regions

<!--more-->

## Desicription

Given a 2D board containing `'X'` and `'O'` (**the letter O**), capture all regions surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

**Example**:

```
X X X X
X O O X
X X O X
X O X X
```

After running your function, the board should be:

```
X X X X
X X X X
X X X X
X O X X
```

**Explanation**:

Surrounded regions shouldn’t be on the border, which means that any `'O'` on the border of the board are not flipped to `'X'`. Any `'O'` that is not on the border and it is not connected to an `'O'` on the border will be flipped to `'X'`. Two cells are connected if they are adjacent cells connected horizontally or vertically.

## Solution

```cpp
class Solution {
private:
    void bfs(int x, int y, vector<vector<char>>& board) {
        int dir[4][2] = {1, 0, 0, 1, -1, 0, 0, -1};
        bool flag = 0;
        queue<pair<int, int>> path;
        vector<pair<int, int>> book;
        path.push(make_pair(x, y));
        book.push_back(make_pair(x, y));
        board[x][y] = 'X';
        if(x == 0 || x == board.size()-1 || y == 0 || y == board[0].size()-1)
            flag = 1;
        while(path.size()) {
            int currentX = path.front().first;
            int currentY = path.front().second;
            path.pop();
            for(int i = 0; i < 4; i++) {
                int dx = currentX + dir[i][0];
                int dy = currentY + dir[i][1];
                if(dx < 0 || dx >= board.size() || dy < 0 || dy >= board[0].size() || board[dx][dy] != 'O')
                    continue;
                path.push(make_pair(dx, dy));
                book.push_back(make_pair(dx, dy));
                board[dx][dy] = 'X';
                if(dx == 0 || dx == board.size()-1 || dy == 0 || dy == board[0].size()-1)
                    flag = 1;
            }
        }
        if(flag) {
            for(auto& it : book)
                board[it.first][it.second] = 'N';
        }
    }
public:
    void solve(vector<vector<char>>& board) {
        for(int i = 0; i < board.size(); i++) {
            for(int j = 0 ; j < board[0].size(); j++) {
                if(board[i][j] == 'O') {
                    bfs(i, j, board);
                }
            }
        }
        for(int i = 0; i < board.size(); i++) {
            for(int j = 0 ; j < board[0].size(); j++) {
                if(board[i][j] == 'N') {
                    board[i][j] = 'O';
                }
            }
        }
    }
};
```