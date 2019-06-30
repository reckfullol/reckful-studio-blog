---
title: LeetCode 0373 - Find K Pairs with Smallest Sums
date: 2019-06-30 21:37:01
categories: LeetCode
---
# Find K Pairs with Smallest Sums

<!--more-->

## Desicription

You are given two integer arrays nums1 and nums2 sorted in ascending order and an integer k.

Define a pair (u,v) which consists of one element from the first array and one element from the second array.

Find the k pairs (u1,v1),(u2,v2) ...(uk,vk) with the smallest sums.

**Example 1:**

```
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]] 
Explanation: The first 3 pairs are returned from the sequence: 
             [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

**Example 2:**

```
Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [1,1],[1,1]
Explanation: The first 2 pairs are returned from the sequence: 
             [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

**Example 3:**

```
Input: nums1 = [1,2], nums2 = [3], k = 3
Output: [1,3],[2,3]
Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]
```

## Solution

```cpp
class Solution {
public:
    std::vector<std::vector<int>> kSmallestPairs(std::vector<int>& nums1, std::vector<int>& nums2, int k) {
        auto cmp = [](std::pair<int, int>a, std::pair<int, int> b) {
            return a.first + a.second < b.first + b.second;
        };
        auto heap = std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, decltype(cmp)>(cmp);
        for(auto num1 : nums1) {
            for(auto num2 : nums2) {
                heap.push({num1, num2});
                if(heap.size() > k) {
                    heap.pop();
                }
            }
        }
        auto res = std::vector<std::vector<int>>(heap.size());
        for(auto& num : res) {
            num = {heap.top().first, heap.top().second};
            heap.pop();
        }
        return res;
    }
};
```