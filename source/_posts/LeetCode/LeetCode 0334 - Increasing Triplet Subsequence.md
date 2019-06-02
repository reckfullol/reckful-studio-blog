---
title: LeetCode 0334 - Increasing Triplet Subsequence
date: 2019-06-02 21:55:14
categories: LeetCode
---
# Increasing Triplet Subsequence

<!--more-->

## Desicription

Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:

Return true if there exists i, j, k 
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.
Note: Your algorithm should run in O(n) time complexity and O(1) space complexity.

**Example 1:**

```
Input: [1,2,3,4,5]
Output: true
```

**Example 2:**

```
Input: [5,4,3,2,1]
Output: false
```

## Solution

```cpp
class Solution {
public:
    bool increasingTriplet(std::vector<int>& nums) {
        int c1 = INT_MAX;
        int c2 = INT_MAX;
        for(auto num : nums) {
            if(num <= c1) {
                c1 = num;
            } else if(num <= c2) {
                c2 = num;
            } else {
                return true;
            }
        }

        return false;
    }
};
```