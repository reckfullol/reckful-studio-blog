---
title: LeetCode 0060 - Permutation Sequence
date: 2017-11-13 16:50:17
categories: LeetCode
---
# Permutation Sequence #

<!--more-->

## Desicription ##

The set `[1,2,3,…,n]` contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):

1. "123"
1. "132"
1. "213"
1. "231"
1. "312"
1. "321"

Given *n* and *k*, return the *kth* permutation sequence.

**Note:** Given *n* will be between 1 and 9 inclusive.

## Solution ##

```cpp
class Solution {
public:
    string getPermutation(int n, int k) {
        string res;
        vector<int> nums(n), f(n);
        for (int i=0; i<n; i++) nums[i]=i+1;
        f[0]=1;
        for (int i=1; i<n; i++) f[i]=f[i-1]*i;
        
        k--;
        for (int i=n-1; i>=0; i--) {
            int j=k/f[i];
            res+=nums[j]+'0';
            nums.erase(nums.begin()+j);
            k%=f[i];
        }
        return res;
     }
};
```