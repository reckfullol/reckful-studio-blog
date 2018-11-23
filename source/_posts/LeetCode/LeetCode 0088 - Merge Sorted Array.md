---
title: LeetCode 0088 - Merge Sorted Array
date: 2017-11-21 10:09:59
categories: LeetCode
---
# Merge Sorted Array #

<!--more-->

## Desicription ##

Given two sorted integer arrays *nums1* and *nums2*, merge nums2 into nums1 as one sorted array.

**Note:**
You may assume that *nums1* has enough space (size that is greater or equal to m + n) to hold additional elements from *nums2*. The number of elements initialized in *nums1* and *nums2* are m and n respectively.

## Solution ##

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int tol = m + n - 1;
        m--;
        n--;
        while(n >= 0)
            nums1[tol--] = (m >= 0 && nums1[m] > nums2[n]) ? nums1[m--] : nums2[n--];        
    }
};
```