---
title: LeetCode 0152 - Maximum Product Subarray
date: 2018-06-02 11:42:10
categories: LeetCode
---
# Maximum Product Subarray

<!--more-->

## Desicription

Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.

**Example** 1:

```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example** 2:

```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

## Solution

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int res = nums[0];
        for(int i = 1, maxNum = res, minNum = res; i < nums.size(); i++) {
            if(nums[i] < 0)
                swap(maxNum, minNum);
            maxNum = max(nums[i], maxNum * nums[i]);
            minNum = min(nums[i], minNum * nums[i]);
            res = max(res, maxNum);
        }
        return res;    
    }
};
```