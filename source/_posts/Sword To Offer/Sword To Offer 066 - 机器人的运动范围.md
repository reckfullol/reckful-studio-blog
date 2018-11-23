---
title: Sword To Offer 066 - 机器人的运动范围
date: 2018-05-15 14:24:17
categories: Sword To Offer
---
# 机器人的运动范围

<!--more-->

## Desicription

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

## Solution

```cpp
class Solution {
private:
    int count = 0;
    int row = 0;
    int col = 0;
    int limit = 0;
    vector<vector<int>> dir = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    vector<vector<bool>> visit;
    void onSearch(int x, int y) {
        int tempX = x;
        int tempY = y;
        int total = 0;
        while(tempX) {
            total += tempX % 10;
            tempX /= 10;
        }
        while(tempY) {
            total += tempY % 10;
            tempY /= 10;
        }
        if(total > limit) {
            return ;
        }
        visit[x][y] = true;
        count++;
        for(int i = 0; i < 4; i++) {
            int dx = x + dir[i][0];
            int dy = y + dir[i][1];
            if(dx < 0 || dx >= row || dy < 0 || dy >= col || visit[dx][dy]) {
                continue;
            }
            onSearch(dx, dy);
        }
    }
public:
    int movingCount(int threshold, int rows, int cols) {
        limit = threshold;
        row = rows;
        col = cols;
        visit = vector<vector<bool>>(row, vector<bool>(col,false));
        onSearch(0, 0);
        return count;
    }
};
```