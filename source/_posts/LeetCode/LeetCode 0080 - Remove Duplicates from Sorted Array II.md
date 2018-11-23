---
title: LeetCode 0080 - Remove Duplicates from Sorted Array II
date: 2017-11-20 21:22:19
categories: LeetCode
---
# Remove Duplicates from Sorted Array II #

<!--more-->

## Desicription ##

Follow up for "Remove Duplicates":

What if duplicates are allowed at most *twice*?

For example,

Given sorted array *nums* = `[1,1,1,2,2,3]`,

Your function should return length = `5`, with the first five elements of nums being `1`, `1`, `2`, `2` and `3`. It doesn't matter what you leave beyond the new length.

## Solution ##

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        map<int, int> cnt;
        for(int i = 0; i < nums.size(); ++i)
            if(cnt[nums[i]] < 2)
                cnt[nums[i]]++;
        nums.clear();
        for(auto it : cnt)
            for(int i = 0; i < it.second; ++i)
                nums.push_back(it.first);
        return nums.size();
    }
};
```