---
title: LeetCode 0349 - Intersection of Two Arrays
date: 2019-06-07 10:56:21
categories: LeetCode
---
# Intersection of Two Arrays

<!--more--> 

## Desicription

Given two arrays, write a function to compute their intersection.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

Note:

- Each element in the result must be unique.
- The result can be in any order.

## Solution

```cpp
class Solution {
public:
    std::vector<int> intersection(std::vector<int>& nums1, std::vector<int>& nums2) {
        auto set1 = std::set<int>();
        auto set2 = std::set<int>();
        for(auto num : nums1) {
            set1.insert(num);
        }

        auto res = std::vector<int>();
        for(auto num : nums2) {
            if(set1.find(num) != set1.end()) {
                set2.insert(num);
            }
        }

        for(auto num : set2) {
            res.push_back(num);
        }
        return res;
    }
};
```