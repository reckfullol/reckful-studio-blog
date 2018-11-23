---
title: LeetCode 0045 - Jump Game II
date: 2017-11-12 20:35:12
categories: LeetCode
---
# Jump Game II #

<!--more-->

## Desicription ##

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

For example:

Given array A = `[2,3,1,1,4]`

The minimum number of jumps to reach the last index is `2`. (Jump `1` step from index 0 to 1, then `3` steps to the last index.)

**Note:**

You can assume that you can always reach the last index.

## Solution ##

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int len = nums.size();
        if(len < 2)
            return 0;
        int step = 0;
        int curFarthest = 0;
        int curEnd = 0;
        for(int i = 0; i < len; i++){
            curFarthest = max(curFarthest, i + nums[i]);
            if(curFarthest >= len - 1)
                return step+1;
            if(i == curEnd)
                step++, curEnd = curFarthest;
        }
        return step;
    }
};
```