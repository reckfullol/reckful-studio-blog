---
title: LeetCode 0327 - Count of Range Sum
date: 2019-05-23 20:25:06
categories: LeetCode
---
# Count of Range Sum

<!--more-->

## Desicription

Given an integer array nums, return the number of range sums that lie in [lower, upper] inclusive.
Range sum S(i, j) is defined as the sum of the elements in nums between indices i and j (i ≤ j), inclusive.

**Note:**

A naive algorithm of O(n2) is trivial. You MUST do better than that.

**Example:**

```
Input: nums = [-2,5,-1], lower = -2, upper = 2,
Output: 3 
Explanation: The three ranges are : [0,0], [2,2], [0,2] and their respective sums are: -2, -1, 2.
```

## Solution

```cpp
class Solution {
public:
    int countRangeSum(std::vector<int>& nums, int lower, int upper) {
        auto sums = std::multiset<long long>();
        int res = 0;
        sums.insert(0);
        long long sum = 0;
        for(auto num : nums) {
            sum += num;
            res += std::distance(sums.lower_bound(sum - upper), sums.upper_bound(sum - lower));
            sums.insert(sum);
        }
        return res;
    }
};
```