---
title: LeetCode 0223 - Rectangle Area
date: 2018-07-04 20:42:49
categories: LeetCode
---
# Rectangle Area

<!--more-->

## Desicription

Find the total area covered by two **rectilinear** rectangles in a **2D** plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

![Rectangle Area](https://leetcode.com/static/images/problemset/rectangle_area.png)

**Example**:

```
Input: A = -3, B = 0, C = 3, D = 4, E = 0, F = -1, G = 9, H = 2
Output: 45
```

**Note**:

Assume that the total area is never beyond the maximum possible value of **int**.

## Solution

```cpp
class Solution {
public:
    long long computeArea(long long A, long long B, long long C, long long D, long long E, long long F, long long G, long long H) {
        long long length_x  = C - A + G - E - (max(C, G) - min(A, E));
        if(length_x <= 0) {
            length_x =  0;
        }
        long long length_y = D - B + H - F - (max(D, H) - min(B, F));
        if(length_y <= 0) {
            length_x = 0;
        }
        return (C -A) * (D - B) + (G - E) * (H - F) - length_x * length_y;
    }
};

```