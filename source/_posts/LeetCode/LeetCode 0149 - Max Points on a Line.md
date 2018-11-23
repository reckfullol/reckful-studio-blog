---
title: LeetCode 0149 - Max Points on a Line
date: 2018-05-29 15:22:54
categories: LeetCode
---
# Max Points on a Line

<!--more-->

## Desicription

Given *n* points on a 2D plane, find the maximum number of points that lie on the same straight line.

**Example** 1:

```
Input: [[1,1],[2,2],[3,3]]
Output: 3
Explanation:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
```

**Example** 2:

```
Input: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4
Explanation:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6
```

## Solution

```cpp
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
class Solution {
public:
    int maxPoints(vector<Point>& points) {
        int res = 0;
        for(int i = 0; i < points.size(); i++) {
            int samePoints = 1;
            int maxNum = 0;
            unordered_map<long double, int> mp;
            for(int j = i+1; j < points.size(); j++) {
                if(points[i].x == points[j].x && points[i].y == points[j].y)
                    samePoints++;
                else if(points[i].x == points[j].x)
                    mp[INT_MAX]++;
                else {
                    long double slope = (long double) (points[j].y - points[i].y) / (long double) (points[j].x - points[i].x);
                    mp[slope]++;
                }
            }
            for(auto it : mp)
                maxNum = max(maxNum, it.second);
            res = max(res, maxNum + samePoints);
        }
        return res;
    }
};
```