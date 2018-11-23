---
title: LeetCode 0167 - Two Sum II - Input array is sorted
date: 2018-06-04 11:51:22
categories: LeetCode
---
# Two Sum II - Input array is sorted

<!--more-->

## Desicription

Given an array of integers that is already **sorted in ascending order**, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

Note:

- Your returned answers (both index1 and index2) are not zero-based.
- You may assume that each input would have exactly one solution and you may not use the same element twice.

**Example**:

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

## Solution

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for(int left = 0, right = numbers.size()-1; left < right; ) {
            if(numbers[left] + numbers[right] > target)
                right--;
            else if(numbers[left] + numbers[right] < target)
                left++;
            else if(numbers[left] + numbers[right] == target)
                return vector<int>{left+1, right+1};
        }
    }
};
```