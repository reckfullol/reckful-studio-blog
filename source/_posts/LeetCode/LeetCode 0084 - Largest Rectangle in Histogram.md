---
title: LeetCode 0084 - Largest Rectangle in Histogram
date: 2017-11-20 21:46:06
categories: LeetCode
---
# Largest Rectangle in Histogram #

<!--more-->

## Desicription ##

Given *n* non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![fuck](https://leetcode.com/static/images/problemset/histogram.png)

Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.

![fuck](https://leetcode.com/static/images/problemset/histogram_area.png)

The largest rectangle is shown in the shaded area, which has area = `10` unit.

For example,

Given heights = `[2,1,5,6,2,3]`,

return `10`.

## Solution ##

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<pair<int, int>> s;
        int res = 0;
        for (int i = 0; i < heights.size(); i++) {
            if (s.empty() || s.top().first < heights[i])
                s.push(make_pair(heights[i], 1));
            else {
                int pre_weight = 0;
                while (s.size() && s.top().first >= heights[i]) {
                    pair<int, int> cur = s.top();
                    s.pop();
                    res = max(res, cur.first * cur.second);
                    if (s.size() && s.top().first >= heights[i])
                        s.top().second += cur.second;
                    pre_weight = cur.second;
                }
                s.push(make_pair(heights[i], 1 + pre_weight));
            }
        }
        while (s.size()) {
            pair<int, int> cur = s.top();
            s.pop();
            if (s.size())
                s.top().second += cur.second;
            res = max(res, cur.first * cur.second);
        }
        return res;
    }
};
```