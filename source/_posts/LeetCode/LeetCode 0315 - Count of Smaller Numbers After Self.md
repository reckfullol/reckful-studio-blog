---
title: LeetCode 0315 - Count of Smaller Numbers After Self
date: 2019-05-15 20:26:05
categories: LeetCode
---
# Count of Smaller Numbers After Self

<!--more-->

## Desicription

You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

**Example:**

```
Input: [5,2,6,1]
Output: [2,1,1,0] 
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

## Solution

```cpp
class Solution {
public:
    std::vector<int> countSmaller(std::vector<int>& nums) {

        auto newNums = std::vector<int>();
        auto res = std::vector<int>(nums.size());

        for(int i = nums.size() - 1; i >= 0; i--) {
            auto distance = std::distance(newNums.begin(), std::lower_bound(newNums.begin(), newNums.end(), nums[i]));
            res[i] = distance;
            newNums.insert(newNums.begin() + distance, nums[i]);
        }

        return res;
    }
};
```