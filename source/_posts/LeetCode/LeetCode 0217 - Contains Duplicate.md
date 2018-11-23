---
title: LeetCode 0217 - Contains Duplicate
date: 2018-06-30 20:41:01
categories: LeetCode
---
# Contains Duplicate

<!--more-->

## Desicription

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

**Example 1:**

```
Input: [1,2,3,1]
Output: true
```

**Example 2:**

```
Input: [1,2,3,4]
Output: false
```

**Example 3:**

```
Input: [1,1,1,3,3,4,3,2,4,2]
Output: true
```

## Solution

```
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> temp_set;
        for(auto num : nums) {
            if(temp_set.find(num) == temp_set.end()) {
                temp_set.insert(num);
            } else {
                return true;
            }
        }
        return false;        
    }
};
```