---
title: LeetCode 0090 - Subsets II
date: 2017-11-22 21:02:37
categories: LeetCode
---
# Subsets II #

<!--more-->

## Desicription ##

Given a collection of integers that might contain duplicates, ***nums***, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

For example,
If ***nums*** = `[1,2,2]`, a solution is:

```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> res = {{}};
        sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size();) {
            int repeat_cnt = 0;
            while(repeat_cnt + i < nums.size() && nums[repeat_cnt+i] == nums[i])
                repeat_cnt++;
            int res_size = res.size();
            for(int j = 0; j < res_size; j++) {
                vector<int> currency = res[j];
                for(int k = 0; k < repeat_cnt; k++){
                    currency.push_back(nums[i]);
                    res.push_back(currency);
                }
            }
            i += repeat_cnt;
        }
        return res;
    }
};
```