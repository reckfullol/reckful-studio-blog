---
title: LeetCode 0001 - Two Sum
date: 2017-11-07 21:07:49
categories: LeetCode
---
# Two Sum #

<!--more-->

## Desicription ##

Given an array of **integers**, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have ***exactly*** one solution, and you may not use the same element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## Solution ##

```cpp
class Solution{
    public:
    vector<int> twoSum(vector<int> &numbers, int target)
    {
        unordered_map<int, int>mp;
        for(int i = 0; i < numbers.size(); i++){
            int need = target - numbers[i];
            if(mp.find(need) != mp.end()){
                vector<int>res;
                res.push_back(i);
                res.push_back(mp[need]);
                return res;
            }
            mp[numbers[i]] = i;
        }
    }
};
```