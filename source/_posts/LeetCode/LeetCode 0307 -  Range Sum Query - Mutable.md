---
title: LeetCode 0307 -  Range Sum Query - Mutable
date: 2019-05-10 23:52:15
categories: LeetCode
---
# Range Sum Query - Mutable

<!--more-->

## Desicription

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.

**Example:**

```

Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8

```

**Note:**

- The array is only modifiable by the update function.
- You may assume the number of calls to update and sumRange function is distributed evenly.

## Solution

```cpp
class NumArray {
    std::vector<int> treeArray;
public:
    NumArray(std::vector<int>& nums) {
        initTreeArray(nums);
    }

    void update(int i, int val) {
        myUpdate(i + 1, val - sumRange(i, i));
    }

    int sumRange(int i, int j) {
        return myQuery(j + 1) - myQuery(i);
    }

private:
    void initTreeArray(std::vector<int>& nums) {
        treeArray = std::vector<int>(nums.size() + 1, 0);
        for(int i = 1; i < treeArray.size(); i++) {
            myUpdate(i, nums[i-1]);
        }
    }

    int lowBit(int i) {
        return i & (-i);
    }

    void myUpdate(int i, int val) {
        for(; i < treeArray.size(); i += lowBit(i)) {
            treeArray[i] += val;
        }
    }

    int myQuery(int i) {
        int res = 0;
        for(; i != 0; i -= lowBit(i)) {
            res += treeArray[i];
        }
        return res;
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * obj->update(i,val);
 * int param_2 = obj->sumRange(i,j);
 */
```