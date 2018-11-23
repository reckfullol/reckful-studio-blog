---
title: LeetCode 0016 - 3Sum Closest
date: 2017-11-09 20:05:55
categories: LeetCode
---
# 3Sum Closest #

<!--more-->

## Desicription ##

Given an array *S* of *n* integers, find three integers in *S* such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

```
For example, given array S = {-1 2 1 -4}, and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

## Solution ##

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if(nums.size() == 3)
            return nums[0] + nums[1] + nums[2];
        std::sort(nums.begin(), nums.end());
        int ans = nums[0] + nums[1] + nums[2];
        for(int i = 0; i < nums.size() - 2; i++){
            int left = i+1;
            int right = nums.size()-1;
            while(left < right){
                int sum = nums[i] + nums[left] + nums[right];
                if(abs(target - sum) < abs(target - ans))
                    ans = sum;
                if(ans == target)
                    return ans;
                if(sum > target)
                    right--;
                else
                    left++;
            }
        }
        return ans;
    }
};
```