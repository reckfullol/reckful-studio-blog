---
title: LeetCode 0229 - Majority Element II
date: 2018-08-17 07:52:42
categories: LeetCode
---
# Majority Element II

<!--more-->

## Desicription

Given an integer array of size *n*, find all elements that appear more than `⌊ n/3 ⌋` times.

**Note:** The algorithm should run in linear time and in O(1) space.

**Example 1:**

```
Input: [3,2,3]
Output: [3]
```

**Example 2:**

```
Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
```

## Solution

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int number_1 = 0;
        int number_2 = 1;
        int count_1 = 0;
        int count_2 = 0;
        for(auto num : nums) {
            if(num == number_1) {
                count_1++;
            } else if(num == number_2) {
                count_2++;
            } else if(count_1 == 0) {
                count_1 = 1;
                number_1 = num;
            } else if(count_2 == 0) {
                count_2 = 1;
                number_2 = num;
            } else {
                count_1--;
                count_2--;
            }
        }
        count_1 = 0;
        count_2 = 0;
        for(auto num : nums) {
            if(num == number_1) {
                count_1++;
            } else if(num == number_2) {
                count_2++;
            }
        }
        vector<int> result;
        if(count_1 > nums.size() / 3) {
            result.push_back(number_1);
        }
        if(count_2 > nums.size() / 3) {
            result.push_back(number_2);
        }
        return result;
    }
};
```