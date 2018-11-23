---
title: LeetCode 0074 - Search a 2D Matrix
date: 2017-11-19 13:44:40
categories: LeetCode
---
# Search a 2D Matrix #

<!--more-->

## Desicription ##

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

For example,

Consider the following matrix:

```
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```

Given **target** = `3`, return `true`.

## Solution ##

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix[0].empty())
            return 0;
        vector<int> col(matrix.size());
        for(int i = 0; i < matrix.size(); ++i)
            col[i] = matrix[i][0];
        int index = lower_bound(col.begin(), col.end(), target) - col.begin();
        if(!index && col[index] > target)
            return 0;
        if(index == matrix.size() ||(index && matrix[index][0] > target))
            --index;
        return *lower_bound(matrix[index].begin(), matrix[index].end(), target) == target;
    }
};
```