---
title: LeetCode 0075 - Sort Colors
date: 2017-11-19 13:51:59
categories: LeetCode
---
# Sort Colors #

<!--more-->

## Desicription ##

Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

**Note:**

You are not suppose to use the library's sort function for this problem.

## Solution ##

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int head = 0;
        int tail = nums.size() - 1;
        for(int i = 0; i <= tail; ++i){
            while(nums[i] == 2 && i < tail)
                swap(nums[i], nums[tail--]);
            while(nums[i] == 0 && i > head)
                swap(nums[i], nums[head++]);
        }
    }
};
```