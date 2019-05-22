---
title: LeetCode 0324 - Wiggle Sort II
date: 2019-05-22 12:25:09
categories: LeetCode
---
# Wiggle Sort II

<!--more-->

## Desicription

Given an unsorted array nums, reorder it such that nums[0] < nums[1] > nums[2] < nums[3]....

**Example 1:**

```
Input: nums = [1, 5, 1, 1, 6, 4]
Output: One possible answer is [1, 4, 1, 5, 1, 6].
```

**Example 2:**

```
Input: nums = [1, 3, 2, 2, 3, 1]
Output: One possible answer is [2, 3, 1, 3, 1, 2].
```

**Note:**

You may assume all input has valid answer.

**Follow Up:**

Can you do it in O(n) time and/or in-place with O(1) extra space?

## Solution

```cpp
class Solution {
public:
    void wiggleSort(std::vector<int>& nums) {
        int n = nums.size();
        auto midIterator = nums.begin() + (n >> 1);
        std::nth_element(nums.begin(), midIterator, nums.end());
        int midValue = *midIterator;

#define reIndex(index) (index << 1 | 1) % (n | 1)
        int index = 0;
        int left = 0;
        int right = n - 1;
        while(index <= right) {
            if(nums[reIndex(index)] > midValue) {
                std::swap(nums[reIndex(left++)], nums[reIndex(index++)]);
            } else if(nums[reIndex(index)] < midValue) {
                std::swap(nums[reIndex(right--)], nums[reIndex(index)]);
            } else {
                index++;
            }
        }

    }
};
```