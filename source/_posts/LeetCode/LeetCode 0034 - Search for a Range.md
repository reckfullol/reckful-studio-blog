---
title: LeetCode 0034 - Search for a Range
date: 2017-11-11 15:15:37
categories: LeetCode
---
# Search for a Range #

<!--more-->

## Desicription ##

Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log *n*).

If the target is not found in the array, return `[-1, -1]`.

For example,
Given `[5, 7, 7, 8, 8, 10]` and target value 8,
return `[3, 4]`. 

## Solution ##

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res;
        auto tmp = lower_bound(nums.begin(), nums.end(), target);
        if(tmp == nums.end() || *tmp != target){
            res.push_back(-1);
            res.push_back(-1);
            return res;
        }
        res.push_back(tmp-nums.begin());
        tmp = upper_bound(nums.begin(), nums.end(), target);
        res.push_back(tmp-nums.begin()-1);
        return res;
    }
};
```