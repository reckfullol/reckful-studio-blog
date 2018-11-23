---
title: LeetCode 0169 - Majority Element
date: 2018-06-04 12:07:41
categories: LeetCode
---
# Majority Element

<!--more-->

## Desicription

Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

**Example** 1:

```
Input: [3,2,3]
Output: 3
```

**Example** 2:

```
Input: [2,2,1,1,1,2,2]
Output: 2
```

## Solution

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int num = 0;
        int cnt = 0;
        for(auto it : nums) {
            if(num != it) {
                cnt--;
                if(cnt < 0)
                    num = it, cnt = 0;
            }
            else
                cnt++;
        }
        return num;
    }
};
```