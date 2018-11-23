---
title: LeetCode 0055 - Jump Game
date: 2017-11-13 16:03:37
categories: LeetCode
---
# Jump Game #

<!--more-->

## Desicription ##

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

For example:

A = `[2,3,1,1,4],` return `true`.

A = `[3,2,1,0,4]`, return `false`.

## Solution ##

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int len = nums.size();
        if(len < 2)
            return 1;
        int curFarthest = 0;
        int curEnd = 0;
        for(int i = 0; i < len; i++){
            if(i > curFarthest)
                return 0;
            curFarthest = max(curFarthest, i + nums[i]);
            if(curFarthest >= len - 1)
                return 1;
            if(i == curEnd)
                curEnd = curFarthest;
        }
        return 0;
    }
};
```