---
title: LeetCode 0053 - Maximum Subarray
date: 2017-11-13 15:52:17
categories: LeetCode
---
# Maximum Subarray #

<!--more-->

## Desicription ##

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array `[-2,1,-3,4,-1,2,1,-5,4]`,
the contiguous subarray `[4,-1,2,1]` has the largest sum = `6`.

## Solution ##

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = INT_MIN;
        int tmp = 0;
        for(int i = 0; i < nums.size(); i++){
            tmp += nums[i];
            res = max(res, tmp);
            if(tmp < 0)
                tmp = 0;
        }
        return res;
    }
};
```