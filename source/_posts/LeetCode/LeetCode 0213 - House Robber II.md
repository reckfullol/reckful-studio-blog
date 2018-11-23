---
title: LeetCode 0213 - House Robber II
date: 2018-06-29 20:10:34
categories: LeetCode
---
# House Robber II

<!--more-->

## Desicription

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle**. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and it **will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Example 1:**

```
Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),
             because they are adjacent houses.
```

**Example 2:**

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

## Solution

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.empty()) {
            return 0;
        }
        if(nums.size() == 1) {
            return nums[0];
        }
        int res1 = 0;
        int res2 = 0;
        int odd = 0;
        int even = 0;
        for(int i = 0; i < nums.size() - 1; i++) {
           if(i & 1) {
               odd = max(even, odd + nums[i]);
           } else {
               even = max(odd, even + nums[i]);
           }
        }
        res1 = max(odd, even);
        odd = 0;
        even = 0;
        for(int i = 1; i < nums.size(); i++) {
            if(i & 1) {
                odd = max(even, odd + nums[i]);
            } else {
                even = max(odd, even + nums[i]);
            }
        }
        res2 = max(odd, even);
        return max(res1, res2);
    }
};
```