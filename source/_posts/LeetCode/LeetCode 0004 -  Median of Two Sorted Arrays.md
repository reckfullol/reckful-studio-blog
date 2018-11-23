---
title: LeetCode 0004 - Median of Two Sorted Arrays
date: 2017-11-07 21:19:49
categories: LeetCode
---
# Median of Two Sorted Arrays #

<!--more-->

## Desicription ##

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

## Solution ##

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int m = nums2.size();
        int p1 = 0;
        int p2 = 0;
        int cnt = 0;
        int res1 = 0;
        int res2 = 0;
        int now = 0;
        while(1){
            if(cnt == (n+m)/2)
                res1 = now;
            else if(cnt == (n+m)/2 + 1){
                res2 = now;
                break;
            }
            if(p1 == n)
                now = nums2[p2++];
            else if(p2 == m)
                now = nums1[p1++];
            else if(nums1[p1] <= nums2[p2])
                now = nums1[p1++];
            else if(nums1[p1] > nums2[p2])
                now = nums2[p2++];
            cnt++;
        }
        if((n + m) & 1)
            return (double) res2;
        else
            return (double) (res1+res2) / 2;
    }
};
```