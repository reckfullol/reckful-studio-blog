---
title: LeetCode 0128 - Longest Consecutive Sequence
date: 2018-05-24 14:58:51
categories: LeetCode
---
# Longest Consecutive Sequence

<!--more-->

## Desicription

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Your algorithm should run in O(*n*) complexity.

**Example**:

```
Input: [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

## Solution

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> unSet(nums.begin(), nums.end());
        int res = 0;
        for(int num : nums) {
            if(unSet.find(num) == unSet.end())
                continue;
            int pre = num - 1;
            int next = num + 1;
            while(unSet.find(pre) != unSet.end())
                unSet.erase(pre--);
            while(unSet.find(next) != unSet.end())
                unSet.erase(next++);
            res = max(res, next - pre - 1);      
        }
        return res;
    }
};
```