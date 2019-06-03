---
title: LeetCode 0335 - Self Crossing
date: 2019-06-03 11:30:05
categories: LeetCode
---
# Self Crossing

<!--more-->

## Desicription

You are given an array x of n positive numbers. You start at point (0,0) and moves x[0] metres to the north, then x[1] metres to the west, x[2] metres to the south, x[3] metres to the east and so on. In other words, after each move your direction changes counter-clockwise.

Write a one-pass algorithm with O(1) extra space to determine, if your path crosses itself, or not.

**Example 1:**

```
┌───┐
│   │
└───┼──>
    │

Input: [2,1,1,2]
Output: true
```

**Example 2:**

```
┌──────┐
│      │
│
│
└────────────>

Input: [1,2,3,4]
Output: false 
```

**Example 3:**

```
┌───┐
│   │
└───┼>

Input: [1,1,1,1]
Output: true 
```

## Solution

```cpp
class Solution {
public:
    bool isSelfCrossing(std::vector<int>& nums) {
        if(nums.size() <= 3) {
            return false;
        }

        for(int i = 3; i < nums.size(); i++) {
            if(nums[i] >= nums[i - 2] && nums[i - 1] <= nums[i - 3]) {
                return true;
            }

            if(i >= 4 && nums[i - 1] == nums[i - 3] && nums[i] + nums[i - 4] >= nums[i - 2]) {
                return true;
            }

            if(i >= 5 && nums[i - 2] >= nums[i - 4] && nums[i] >= nums[i - 2] - nums[i - 4] && nums[i - 1] >= nums[i - 3] - nums[i - 5] && nums[i - 1] && nums[i - 1] <= nums[i - 3]) {
                return true;
            }
        }

        return false;
    }
};
```