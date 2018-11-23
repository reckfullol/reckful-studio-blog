---
title: LeetCode 0219 - Contains Duplicate II
date: 2018-07-02 11:32:44
categories: LeetCode
---
# Contains Duplicate II

<!--more-->

## Desicription

Given an array of integers and an integer k, find out whether there are two distinct indices *i* and *j* in the array such that **nums[i] = nums[j]** and the absolute difference between *i* and *j* is at most k.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1
Output: true
```

**Example 3:**

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

## Solution

```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, vector<int>> book;
        for(int i = 0; i < nums.size(); i++) {
            if(book.find(nums[i]) == book.end()) {
                book[nums[i]] = vector<int>();
            }
            book[nums[i]].push_back(i);
        }
        for(const auto& num : book) {
            for(int i = 1; i < num.second.size(); i++) {
                if(num.second[i] - num.second[i - 1] <= k) {
                    return true;
                }
            }
        }
        return false;
    }
};
```