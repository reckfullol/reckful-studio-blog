---
title: LeetCode 0391 - Perfect Rectangle
date: 2019-09-04 12:36:43
categories: LeetCode
---
# Perfect Rectangle

<!--more-->

## Desicription

Given N axis-aligned rectangles where N > 0, determine if they all together form an exact cover of a rectangular region.

Each rectangle is represented as a bottom-left point and a top-right point. For example, a unit square is represented as [1,1,2,2]. (coordinate of bottom-left point is (1, 1) and top-right point is (2, 2)).

**Example 1:**

```
rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [3,2,4,4],
  [1,3,2,4],
  [2,3,3,4]
]

Return true. All 5 rectangles together form an exact cover of a rectangular region.
```

**Example 2:**

```
rectangles = [
  [1,1,2,3],
  [1,3,2,4],
  [3,1,4,2],
  [3,2,4,4]
]

Return false. Because there is a gap between the two rectangular regions.
```

**Example 3:**

```
rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [1,3,2,4],
  [3,2,4,4]
]

Return false. Because there is a gap in the top center.
```

**Example 4:**

```
rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [1,3,2,4],
  [2,2,4,4]
]

Return false. Because two of the rectangles overlap with each other.
```

## Solution

```cpp
class Solution {
public:
    bool isRectangleCover(const std::vector<std::vector<int>>& rectangles) {
        int left = INT_MAX;
        int right = INT_MIN;
        int top = INT_MIN;
        int bottom = INT_MAX;
        auto set = std::set<std::pair<int, int>>{};
        int sumArea = 0;
        for(auto& rectangle : rectangles) {
            left = std::min(left, rectangle[0]);
            right = std::max(right, rectangle[2]);
            top = std::max(top, rectangle[3]);
            bottom = std::min(bottom, rectangle[1]);

            static const auto map = std::vector<std::pair<int, int>> {
                    {0, 1},
                    {2, 3},
                    {2, 1},
                    {0, 3},
            };

            for(const auto& it : map) {
                std::pair<int, int> currentPoint = {rectangle[it.first], rectangle[it.second]};
                if(set.find(currentPoint) != set.end()) {
                    set.erase(currentPoint);
                } else {
                    set.insert(currentPoint);
                }
            }
            sumArea += (rectangle[2] - rectangle[0]) * (rectangle[3] - rectangle[1]);
        }

        return !(set.size() != 4 || set.find({left, top}) == set.end() || set.find({left, bottom}) == set.end() || set.find({right, top}) == set.end() || set.find({right, bottom}) == set.end() || sumArea != ((right - left) * (top - bottom)));
    }
};
```