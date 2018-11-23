---
title: LeetCode 0220 - Contains Duplicate III
date: 2018-07-02 17:36:50
categories: LeetCode
---
# Contains Duplicate III

<!--more-->

## Desicription

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the **absolute** difference between **nums[i]** and **nums[j]** is at most t and the **absolute** difference between i and j is at most k.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```

**Example 3:**

```
Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false
```

## Solution

```cpp
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        map<long long, long long> book;
        if (k < 1 || t < 0) {
            return false;
        }
        for(int i = 0; i < nums.size(); i++) {
            long long sub = nums[i] - LONG_LONG_MIN;
            long long index = sub / (t + 1);
            if(book.find(index) != book.end() ||
                    (book.find(index - 1) != book.end() && sub - book[index - 1] <= t) ||
                    (book.find(index + 1) != book.end() && book[index + 1] - sub <= t)) {
                return true;
            }
            if(book.size() >= k) {
                book.erase(book.find((nums[i - k] - LONG_LONG_MIN) / (t + 1)));
            }
            book[index] = sub;
        }
        return false;
    }
};
```