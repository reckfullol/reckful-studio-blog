---
title: LeetCode 0118 - Pascal's Triangle
date: 2017-12-18 13:16:11
categories: LeetCode
---
# Pascal's Triangle #

<!--more-->

## Desicription ##

Given `numRows`, generate the first `numRows` of Pascal's triangle.

For example, given `numRows` = 5,
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> res(numRows, vector<int>());
        for(int i = 0; i < numRows; i++) {
            res[i].push_back(1);
            for(int j = 1; j < i; j++)
                res[i].push_back(res[i-1][j-1] + res[i-1][j]);
            if(i)
                res[i].push_back(1);
        }
        return res;
    }
};
```