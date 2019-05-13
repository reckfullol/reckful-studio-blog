---
title: LeetCode 0312 - Burst Balloons
date: 2019-05-13 23:30:34
categories: LeetCode
---
# Burst Balloons

<!--more-->

## Desicription

Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

**Note:**

- You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
- 0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

**Example:**

```
Input: [3,1,5,8]
Output: 167 
Explanation: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
             coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

## Solution

```cpp
class Solution {
public:
    int maxCoins(std::vector<int>& nums) {
        auto balloons = std::vector<int>(nums.size() + 2);
        balloons[0] = balloons[balloons.size() - 1] = 1;
        for(int i = 1; i < balloons.size() - 1; i++) {
            balloons[i] = nums[i - 1];
        }
        
        auto dp = std::vector<std::vector<int>>(balloons.size(), std::vector<int>(balloons.size(), 0));
        for(int length = 2; length < balloons.size(); length++) {
            for(int left = 0; left + length < balloons.size(); left++) {
                int right = left + length;
                for(int i = left + 1; i < right; i++) {
                    dp[left][right] = std::max(dp[left][right], balloons[left] * balloons[i] * balloons[right] + dp[left][i] + dp[i][right]);
                }
            }
        }
        
        return dp[0][balloons.size() -1];
    }
};
```