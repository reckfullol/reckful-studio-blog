---
title: LeetCode 0081 - Search in Rotated Sorted Array II
date: 2017-11-20 21:26:39
categories: LeetCode
---
# Search in Rotated Sorted Array II #

<!--more-->

## Desicription ##

Follow up for "Search in Rotated Sorted Array":

What if duplicates are allowed?

Would this affect the run-time complexity? How and why?

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

Write a function to determine if a given target is in the array.

The array may contain duplicates.

## Solution ##

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        while(left <= right){
            int mid = (left + right) >> 1;
            if(nums[mid] == target)
                return 1;
            while(nums[left] == nums[mid] && nums[right] == nums[mid])
                left++, right--;
            if(nums[mid] >= nums[left]){
                if(nums[left] <= target && nums[mid] > target)
                    right = mid - 1;
                else
                    left = mid + 1;
            }
            else{
                if(nums[right] >= target && nums[mid] < target)
                    left = mid + 1;
                else
                    right = mid - 1;
            }
        }
        return 0;
    }
};
```