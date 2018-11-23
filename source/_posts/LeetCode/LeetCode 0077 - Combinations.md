---
title: LeetCode 0077 - Combinations
date: 2017-11-19 14:36:21
categories: LeetCode
---
# Combinations #

<!--more-->

## Desicription ##

Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.

For example,

If *n* = 4 and *k* = 2, a solution is:

```
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> res;
        vector<int> tmp(k, 0);
        int index = 0;
        while(index >= 0){
            tmp[index]++;
            if(tmp[index] > n)
                index--;
            else if(index == k - 1)
                res.push_back(tmp);
            else
                index++, tmp[index] = tmp[index - 1];
        }
        return res;
    }
};
```