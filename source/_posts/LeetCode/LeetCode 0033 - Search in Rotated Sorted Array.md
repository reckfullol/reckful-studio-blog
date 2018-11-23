---
title: LeetCode 0033 - Search in Rotated Sorted Array
date: 2017-11-11 15:06:13
categories: LeetCode
---
# Search in Rotated Sorted Array #

<!--more-->

## Desicription ##

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

## Solution ##

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size()-1;
        while(left <= right){
            int mid = (left + right) >> 1;
            if(nums[mid] == target)
                return mid;
            if(nums[mid] < nums[right]){
                if(nums[mid] < target && nums[right] >= target)
                    left = mid + 1;
                else
                    right = mid - 1;
            }
            else{
                if(nums[mid] > target && nums[left] <= target)
                    right = mid - 1;
                else
                    left = mid + 1;
            }
        }
        return -1;
    }
};
```