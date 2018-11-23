---
title: LeetCode 0153 - Find Minimum in Rotated Sorted Array
date: 2018-06-02 11:45:25
categories: LeetCode
---
# Find Minimum in Rotated Sorted Array

<!--more-->

## Desicription

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

You may assume no duplicate exists in the array.

**Example** 1:

```
Input: [3,4,5,1,2] 
Output: 1
```

**Example** 2:

```
Input: [4,5,6,7,0,1,2]
Output: 0
```

## Solution

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0;
        int right = nums.size()-1;
        while(left < right) {
            if(nums[left] < nums[right])
                return nums[left];
            int mid = (left + right) >> 1;
            if(nums[left] <= nums[mid])
                left = mid + 1;
            else
                right = mid;
        }
        return nums[left];
    }
};
```