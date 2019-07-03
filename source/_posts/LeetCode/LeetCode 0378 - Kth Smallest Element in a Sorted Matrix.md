---
title: LeetCode 0378 - Kth Smallest Element in a Sorted Matrix
date: 2019-07-03 11:22:48
categories: LeetCode
---
# Kth Smallest Element in a Sorted Matrix

<!--more-->

## Desicription


Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

**Example:**

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```

**Note:**


You may assume k is always valid, 1 ≤ k ≤ n^2.

## Solution

```cpp
class Solution {
public:
    int kthSmallest(std::vector<std::vector<int>>& matrix, int k) {
        auto nums = std::vector<int>();
        for(auto& rows : matrix) {
            for(auto& num : rows) {
                nums.push_back(num);
            }
        }
        std::sort(nums.begin(), nums.end());
        return nums[k - 1];
    }
};
```