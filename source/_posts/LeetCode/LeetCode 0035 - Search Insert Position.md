---
title: LeetCode 0035 - Search Insert Position
date: 2017-11-11 15:17:42
categories: LeetCode
---
# Search Insert Position #

<!--more-->

## Desicription ##

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

**Example 3:**

```
Input: [1,3,5,6], 7
Output: 4
```

**Example 1:**

```
Input: [1,3,5,6], 0
Output: 0
```

## Solution ##

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        return lower_bound(nums.begin(), nums.end(), target) - nums.begin();
    }
};
```