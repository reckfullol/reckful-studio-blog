---
title: Sword To Offer 065 - 矩阵中的路径
date: 2018-05-15 12:23:55
categories: Sword To Offer
---
# 矩阵中的路径

<!--more-->

## Desicription

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。 例如 a b c e s f c s a d e e 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

## Solution

```cpp
class Solution {
private:
    vector<vector<bool>> visit;
    vector<vector<int>> dir = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    vector<vector<char>> mat;
    unsigned int row;
    unsigned int col;
    string findString;
    bool found = false;
    void onSearch(int x, int y, int index) {
        if(found) {
            return ;
        }
        if(index == findString.size() -1) {
            found = true;
            return ;
        }
        visit[x][y] = true;
        for(int i = 0; i < 4; i++) {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if(dx < 0 || dx >= row || dy < 0 || dy >= col || mat[dx][dy] != findString[index + 1] || visit[dx][dy]) {
                continue;
            }
            onSearch(dx, dy, index + 1);
        }

    }
public:
    bool hasPath(const char* matrix, int rows, int cols, char* str) {
        findString = str;
        row = static_cast<unsigned int>(rows);
        col = static_cast<unsigned int>(cols);
        mat = vector<vector<char>>(row, vector<char>(col));
        int index = 0;
        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                mat[i][j] = matrix[index++];
            }
        }
        visit = vector<vector<bool>>(row, vector<bool>(col, false));
        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                if(found) {
                    break;
                }
                if(mat[i][j] != findString[0]) {
                    continue;
                }
                onSearch(i, j, 0);
                for(int x = 0; x < row; x++) {
                    for(int y = 0; y < col; y++) {
                        visit[x][y] = false;
                    }
                }
            }
            if(found) {
                break;
            }
        }
        return found;
    }
};
```