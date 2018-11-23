---
title: LeetCode 0018 - 4Sum
date: 2017-11-09 20:11:12
categories: LeetCode
---
# 4Sum #

<!--more-->

## Desicription ##

Given an array *S* of n integers, are there elements *a, b, c,* and *d* in *S* such that *a + b + c + d = target*? Find all unique quadruplets in the array which gives the sum of target.

**Note:** The solution set must not contain duplicate quadruplets.

```
For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> res;
        if(nums.size() <= 3)
            return res;
        std::sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size(); i++){
            int target3 = target - nums[i];
            for(int j = i + 1; j < nums.size(); j++){
                int target2 = target3 - nums[j];
                int left = j + 1;
                int right = nums.size() - 1;
                while(left < right){
                    int sum = nums[left] + nums[right];
                    if(sum < target2)
                        left++;
                    else if(sum > target2)
                        right--;
                    else{
                        vector<int> tmp;
                        tmp.push_back(nums[i]);
                        tmp.push_back(nums[j]);
                        tmp.push_back(nums[left]);
                        tmp.push_back(nums[right]);
                        res.push_back(tmp);
                        while(left < right && nums[left] == tmp[2])
                            left++;
                        while(left < right && nums[right] == tmp[3])
                            right--;
                    }
                }
                while(j + 1 < nums.size() && nums[j+1] == nums[j])
                    j++;
            }
            while(i + 1 < nums.size() && nums[i+1] == nums[i])
                i++;
        }
        return res;
    }
};
```