---
title: LeetCode 0132 - Palindrome Partitioning II
date: 2018-05-25 20:43:42
categories: LeetCode
---
# Palindrome Partitioning II

<!--more-->

## Desicription


Given a string *s*, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of *s*.

Example:

```
Input: "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

## Solution

```cpp
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        vector<int> cur(n+1);
        for(int i = 0; i <= n; i++)
            cur[i] = i-1;
        for(int i = 0; i < n; i++) {
            for(int j = 0; i-j >= 0 && i+j < n && s[i-j] == s[i+j]; j++)
                cur[i+j+1] = min(cur[i+j+1], 1+cur[i-j]);
            for(int j = 1; i-j+1 >= 0 && i+j < n && s[i-j+1] == s[i+j]; j++)
                cur[i+j+1] = min(cur[i+j+1], 1+cur[i-j+1]);
        }
        return cur[n];  
    }
};
```