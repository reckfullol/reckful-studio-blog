---
title: LeetCode 0384 - Shuffle an Array
date: 2019-08-13 23:56:45
categories: LeetCode
---
# Shuffle an Array

<!--more-->

## Desicription

Shuffle a set of numbers without duplicates.

**Example:**

```
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```

## Solution

```cpp
class Solution {
private:
    std::vector<int> nums;

public:
    Solution(std::vector<int>& nums) {
        this->nums = nums;
    }
    
    /** Resets the array to its original configuration and return it. */
    std::vector<int> reset() {
        return nums;
    }
    
    /** Returns a random shuffling of the array. */
    std::vector<int> shuffle() {
        auto ret = std::vector<int>(nums);
        for(int i = 0; i < ret.size(); i++) {
            int index = rand() % (ret.size() - i);
            std::swap(ret[i], ret[i + index]);
        }
        return ret;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(nums);
 * vector<int> param_1 = obj->reset();
 * vector<int> param_2 = obj->shuffle();
 */
```