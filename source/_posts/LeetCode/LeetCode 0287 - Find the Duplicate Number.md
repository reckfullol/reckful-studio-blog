---
title: LeetCode 0287 - Find the Duplicate Number
date: 2018-12-18 17:06:30
categories: LeetCode
---
# Find the Duplicate Number

<!--more-->

## Desicription

Given an array *nums* containing *n* + 1 integers where each integer is between 1 and *n* (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

**Example 1:**

```
Input: [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: [3,1,3,4,2]
Output: 3
```

**Note:**

1. You must not modify the array (assume the array is read only).
2. You must use only constant, O(1) extra space.
3. Your runtime complexity should be less than O(n^2).
4. There is only one duplicate number in the array, but it could be repeated more than once.

## Solution

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        if(nums.size() <= 1) {
            return 0;
        }
        int slow = nums[0];
        int first = nums[nums[0]];
        while(slow != first) {
            slow = nums[slow];
            first = nums[nums[first]];
        }
        first = 0;
        while(first != slow) {
            slow = nums[slow];
            first = nums[first];
        }
        return slow;
    }
};
```