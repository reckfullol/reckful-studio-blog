---
title: LeetCode 0078 - Subsets
date: 2017-11-19 15:05:50
categories: LeetCode
---
# Subsets #

<!--more-->

## Desicription ##

Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

For example,

If **nums** = `[1,2,3]`, a solution is:

```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int nums_size = nums.size();
        int res_size = pow(2, nums_size);
        vector<vector<int>> res(res_size);
        for(int i = 0; i < nums_size; i++)
            for(int j = 0; j < res_size; j++)
                if((j >> i) & 1)
                    res[j].push_back(nums[i]);
        return res;
    }
};
```