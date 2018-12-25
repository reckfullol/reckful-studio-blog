---
title: LeetCode 0303 - Range Sum Query - Immutable
date: 2018-12-25 15:32:34
categories: LeetCode
---
# Range Sum Query - Immutable

<!--more-->

## Desicription

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

**Example:**

```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

**Note:**

1. You may assume that the array does not change.
2. There are many calls to sumRange function.

## Solution

```cpp
class NumArray {
private:
    vector<int> pre;
public:
    explicit NumArray(vector<int> nums) {
        pre = vector<int>(nums.size(), 0);
        for(int i = 0; i < pre.size(); i++) {
            if(i == 0) {
                pre[i] = nums[i];
            } else {
                pre[i] = pre[i - 1] + nums[i];
            }
        }
    }

    int sumRange(int i, int j) {
        return pre[j] - (i == 0 ? 0 : pre[i - 1]);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```