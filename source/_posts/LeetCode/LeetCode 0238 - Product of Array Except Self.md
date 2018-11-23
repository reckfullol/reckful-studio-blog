---
title: LeetCode 0238 - Product of Array Except Self
date: 2018-09-18 00:04:29
categories: LeetCode
---
# Product of Array Except Self

<!--more-->

## Desicription

Given an array `nums` of n integers where n > 1,  return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

**Example:**

```
Input:  [1,2,3,4]
Output: [24,12,8,6]
```

**Note**: Please solve it **without division** and in O(n).

**Follow up:**
Could you solve it with constant space complexity? (The output array **does not** count as extra space for the purpose of space complexity analysis.)

## Solution

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> pre(nums.size(), 1);
        vector<int> suf(nums.size(), 1);
        vector<int> res(nums.size(), 1);
        
        for(int i = 0; i < nums.size(); i++) {
            pre[i] = i ? pre[i - 1] * nums[i] : nums[i];
        }
        for(int i = nums.size() - 1; i > 0; i--) {
            suf[i] = i == nums.size() - 1 ? nums[i] : suf[i + 1] * nums[i];
        }
        for(int i = 0; i < nums.size(); i++) {
            if(i == 0) {
                res[i] = suf[i + 1];
            } else if(i == nums.size() - 1) {
                res[i] = pre[i - 1];
            } else {
                res[i] = pre[i - 1] * suf[i + 1];
            }
        }
        return res;
    }
};
```