---
title: LeetCode 0350 - Intersection of Two Arrays II
date: 2019-06-07 11:13:54
categories: LeetCode
---
# Intersection of Two Arrays II

<!--more-->

## Desicription

Given two arrays, write a function to compute their intersection.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```

Note:

- Each element in the result should appear as many times as it shows in both arrays.
- The result can be in any order.

Follow up:

- What if the given array is already sorted? How would you optimize your algorithm?
- What if nums1's size is small compared to nums2's size? Which algorithm is better?
- What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

## Solution

```cpp
class Solution {
public:
    std::vector<int> intersect(std::vector<int>& nums1, std::vector<int>& nums2) {
        auto count1 = std::unordered_map<int, int>();
        auto count2 = std::unordered_map<int, int>();

        for(auto num : nums1) {
            count1[num]++;
        }
        for(auto num : nums2) {
            count2[num]++;
        }

        auto res = std::vector<int>();
        for(auto num : count1) {
            if(count2.find(num.first) != count2.end()) {
                for(int i = 0; i < std::min(num.second, count2[num.first]); i++) {
                    res.push_back(num.first);
                }
            }
        }

        return res;
    }
};
```