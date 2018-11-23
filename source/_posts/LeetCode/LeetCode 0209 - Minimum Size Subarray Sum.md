---
title: LeetCode 0209 - Minimum Size Subarray Sum
date: 2018-06-12 16:57:51
categories: LeetCode
---
# Minimum Size Subarray Sum

<!--more-->

## Desicription

Given an array of **n** positive integers and a positive integer **s**, find the minimal length of a **contiguous** subarray of which the sum ≥ **s**. If there isn't one, return 0 instead.

**Example**: 

```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

**Follow up**:

If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n). 

## Solution

```cpp
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int left = 0;
        int right = 0;
        int res = INT_MAX;
        int sum = 0;
        while(right < nums.size()) {
            sum += nums[right++];
            while(sum >= s) {
                res = min(res, right - left);
                sum -= nums[left++];
            }
        }
        return res==INT_MAX?0:res;
    }
};  
```