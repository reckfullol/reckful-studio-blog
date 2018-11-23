---
title: LeetCode 0042 - Trapping Rain Water
date: 2017-11-11 21:23:33
categories: LeetCode
---
# Trapping Rain Water #

<!--more-->

## Desicription ##

Given *n* non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example, 

Given `[0,1,0,2,1,0,1,3,2,1,2,1]`, return `6`.

![fuck](https://leetcode.com/static/images/problemset/rainwatertrap.png)

*The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped. Thanks Marcos for contributing this image!*

## Solution ##

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if(!height.size())
            return 0;
        int left = 0;
        int right = height.size()-1;
        int left_max = height[left];
        int right_max = height[right];
        int res = 0;
        while(left <= right){
            if(height[left] <= height[right]){
                if(height[left] > left_max)
                    left_max = height[left];
                else
                    res += left_max - height[left];
                left++;
            }
            else{
                if(height[right] > right_max)
                    right_max = height[right];
                else
                    res += right_max - height[right];
                right--;
            }
        }
        return res;
    }
};
```