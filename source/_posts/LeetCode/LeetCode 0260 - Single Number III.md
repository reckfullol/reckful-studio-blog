---
title: LeetCode 0260 - Single Number III
date: 2018-12-11 21:43:53
categories: LeetCode
---
# Single Number III

<!--more-->

## Desicription

Given an array of numbers `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

**Example:**

```
Input:  [1,2,1,3,2,5]
Output: [3,5]
```

**Note:**

1. The order of the result is not important. So in the above example, [5, 3] is also correct.
2. Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

## Solution

```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int ret = 0;
        for(const auto& num : nums) {
            ret ^= num;
        }
        ret &= -ret;
        int ans_1 = 0;
        int ans_2 = 0;
        for(const auto& num : nums) {
            if((num & ret) > 0) {
                ans_1 ^= num;
            } else {
                ans_2 ^= num;
            }
        }
        return vector<int>{ans_1, ans_2};
    }
};
```