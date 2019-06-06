---
title: LeetCode 0347 - Top K Frequent Elements
date: 2019-06-06 22:54:07
categories: LeetCode
---
# Top K Frequent Elements

<!--more-->

## Desicription

Given a non-empty array of integers, return the k most frequent elements.

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

Note:

You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

## Solution

```cpp
class Solution {
public:
    std::vector<int> topKFrequent(std::vector<int>& nums, int k) {
        auto frequents = std::unordered_map<int, int>();
        for(auto num : nums) {
            frequents[num]++;
        }

        auto cmp = [](std::pair<int, int> a, std::pair<int, int> b) {
            return a.second > b.second;
        };
        auto heap = std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, decltype(cmp)>(cmp);
        for(auto frequent : frequents) {
            heap.push({frequent.first, frequent.second});
            while(heap.size() > k) {
                heap.pop();
            }
        }

        auto res = std::vector<int>();
        while(!heap.empty()) {
            res.push_back(heap.top().first);
            heap.pop();
        }
        return res;
    }
};
```