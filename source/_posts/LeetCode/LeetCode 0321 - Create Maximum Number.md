---
title: LeetCode 0323 - Create Maximum Number
date: 2019-05-19 22:25:48
categories: LeetCode
---
# Create Maximum Number

<!--more-->

## Desicription

Given two arrays of length m and n with digits 0-9 representing two numbers. Create the maximum number of length k <= m + n from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the k digits.

**Note:** You should try to optimize your time and space complexity.

**Example 1:**

```
Input:
nums1 = [3, 4, 6, 5]
nums2 = [9, 1, 2, 5, 8, 3]
k = 5
Output:
[9, 8, 6, 5, 3]
```

**Example 2:**

```
Input:
nums1 = [6, 7]
nums2 = [6, 0, 4]
k = 5
Output:
[6, 7, 6, 0, 4]
```

**Example 3:**

```
Input:
nums1 = [3, 9]
nums2 = [8, 9]
k = 3
Output:
[9, 8, 9]
```

## Solution

```cpp
class Solution {
public:
    std::vector<int> maxNumber(const std::vector<int>& nums1, const std::vector<int>& nums2, int k) const {
        auto res = std::vector<int>();
        for(int k1 = std::max(0, k - (int)nums2.size()); k1 <= std::min(k, (int)nums1.size()); k1++) {
            res = std::max(res, maxNumber(maxNumber(nums1, k1), maxNumber(nums2, k - k1)));
        }

        return res;
    }

    std::vector<int> maxNumber(const std::vector<int>& nums, int k) const {
        int dropCount = nums.size() - k;
        auto stack = std::stack<int>();
        for(auto num : nums) {
            while(dropCount != 0 && !stack.empty() && stack.top() < num) {
                stack.pop();
                dropCount--;
            }
            stack.push(num);
        }

        auto res = std::vector<int>(k);
        while(stack.size() > k) {
            stack.pop();
        }
        for(int i = res.size() - 1; i >= 0; i--) {
            res[i] = stack.top();
            stack.pop();
        }
        return res;
    }

    std::vector<int> maxNumber(const std::vector<int>& nums1, const std::vector<int>& nums2) const {
        auto res = std::vector<int>(nums1.size() + nums2.size());
        auto it1 = nums1.begin();
        auto it2 = nums2.begin();
        for(auto& num : res) {
            num = std::lexicographical_compare(it1, nums1.end(), it2, nums2.end()) ? *it2++ : *it1++;
        }
        return res;
    }
};
```