---
title: LeetCode 0368 - Largest Divisible Subset
date: 2019-06-30 17:10:56
categories: LeetCode
---
# Largest Divisible Subset

<!--more-->

## Desicription

Given a set of distinct positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies:

Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

**Example 1:**

```
Input: [1,2,3]
Output: [1,2] (of course, [1,3] will also be ok)
```

**Example 2:**

```
Input: [1,2,4,8]
Output: [1,2,4,8]
```

## Solution

```cpp
class Solution {
public:
    std::vector<int> largestDivisibleSubset(std::vector<int>& nums) {
        std::sort(nums.begin(), nums.end());

        auto dp = std::vector<int>(nums.size(), 0);
        auto parent = std::vector<int>(nums.size(), 0);

        int maxNum = INT_MIN;
        int maxIndex = 0;

        for(int i = nums.size() - 1; i >= 0; i--) {
            for(int j = i; j < nums.size(); j++) {
                if(nums[j] % nums[i] == 0 && dp[i] < dp[j] + 1) {
                    dp[i] = dp[j] + 1;
                    parent[i] = j;
                }
            }

            if(dp[i] > maxNum) {
                maxNum = dp[i];
                maxIndex = i;
            }
        }

        auto res = std::vector<int>();
        for(int i = 0; i < maxNum; i++) {
            res.push_back(nums[maxIndex]);
            maxIndex = parent[maxIndex];
        }

        return res;
    }
};
```