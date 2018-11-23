---
title: LeetCode 0046 - Permutations
date: 2017-11-12 20:52:59
categories: LeetCode
---
# Permutations #

<!--more-->

## Desicription ##

Given a collection of **distinct** numbers, return all possible permutations.

For example,

`[1,2,3]` have the following permutations:

```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        do
            res.push_back(nums);
        while(next_permutation(nums.begin(), nums.end()));
        return res;
    }
};
```