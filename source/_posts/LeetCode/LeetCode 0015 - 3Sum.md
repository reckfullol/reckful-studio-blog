---
title: LeetCode 0015 - 3Sum
date: 2017-11-09 20:02:41
categories: LeetCode
---
# 3Sum #

<!--more-->

## Desicription ##

Given an array *S* of *n* integers, are there elements *a, b, c* in *S* such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:** The solution set must not contain duplicate triplets.

```
For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        std::sort(nums.begin(), nums.end());
        vector<vector<int>>res;
        for(int i = 0; i < nums.size(); i++){
            int target = -nums[i];
            int left = i+1;
            int right = nums.size()-1;
            while(left < right){
                if(nums[left] + nums[right] < target)
                    left++;
                else if(nums[left] + nums[right] > target)
                    right--;
                else{
                    vector<int> tri(3, 0);
                    tri[0] = -target;
                    tri[1] = nums[left];
                    tri[2] = nums[right];
                    res.push_back(tri);
                    while(tri[1] == nums[left] && left < right)
                        left++;
                    while(tri[2] == nums[right] && left < right)
                        right--;
                }
            }
            while (i + 1 < nums.size() && nums[i + 1] == nums[i])
                i++;
        }
        return res;
    }
};
```