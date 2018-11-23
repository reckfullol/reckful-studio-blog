---
title: LeetCode 0218 - The Skyline Problem
date: 2018-07-01 20:13:22
categories: LeetCode
---
# The Skyline Problem

<!--more-->

## Desicription

A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are **given the locations and height of all the buildings** as shown on a cityscape photo (Figure A), write a program to **output the skyline** formed by these buildings collectively (Figure B).

![Buildings](https://leetcode.com/static/images/problemset/skyline1.jpg)  ![Skyline Contour](https://leetcode.com/static/images/problemset/skyline2.jpg)

The geometric information of each building is represented by a triplet of integers `[Li, Ri, Hi]`, where `Li` and `Ri` are the x coordinates of the left and right edge of the ith building, respectively, and Hi is its height. It is guaranteed that `0 ≤ Li, Ri ≤ INT_MAX`, `0 < Hi ≤ INT_MAX`, and `Ri - Li > 0`. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

For instance, the dimensions of all buildings in Figure A are recorded as: `[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ]` .

The output is a list of "**key points**" (red dots in Figure B) in the format of `[ [x1,y1], [x2, y2], [x3, y3], ... ]` that uniquely defines a skyline. **A key point is the left endpoint of a horizontal line segment**. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

For instance, the skyline in Figure B should be represented as:`[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]`.

**Notes**:

- The number of buildings in any input list is guaranteed to be in the range `[0, 10000]`.

- The input list is already sorted in ascending order by the left x position `Li`.

- The output list must be sorted by the x position.

- There must be no consecutive horizontal lines of equal height in the output skyline. For instance, `[...[2 3], [4 5], [7 5], [11 5], [12 7]...]` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: `[...[2 3], [4 5], [12 7], ...]`

## Solution

```cpp
class Solution {
public:
    vector<pair<int, int>> getSkyline(vector<vector<int>>& buildings) {
        vector<pair<int, int>> result;

        int current_index = 0;
        int current_x;
        int current_height;
        priority_queue<pair<int, int>> left_building;

        while(current_index < buildings.size() || !left_building.empty()) {

            current_x = left_building.empty() ? buildings[current_index][0] : left_building.top().second;

            if(current_index >= buildings.size() || current_x < buildings[current_index][0]) {
                while(!left_building.empty() && (current_x >= left_building.top().second)) {
                    left_building.pop();
                }
            } else {
                current_x = buildings[current_index][0];
                while(current_index < buildings.size() && current_x == buildings[current_index][0]) {
                    left_building.push(pair<int, int>(buildings[current_index][2], buildings[current_index][1]));
                    current_index++;
                }
            }

            current_height = left_building.empty() ? 0 : left_building.top().first;

            if(result.empty() || (result.back().second != current_height)) {
                result.emplace_back(pair<int, int>(current_x, current_height));
            }
        }

        return result;
    }
};
```