---
title: LeetCode 0047 - Permutations II
date: 2017-11-12 20:57:11
categories: LeetCode
---
# Permutations II #

<!--more-->

## Desicription ##

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

For example,

`[1,1,2]` have the following unique permutations:

```
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        do
            res.push_back(nums);
        while(next_permutation(nums.begin(), nums.end()));
        return res;
    }
};
```