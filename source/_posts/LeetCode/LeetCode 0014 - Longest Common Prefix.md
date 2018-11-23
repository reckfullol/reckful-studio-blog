---
title: LeetCode 0014 - Longest Common Prefix
date: 2017-11-09 19:50:01
categories: LeetCode
---
# Longest Common Prefix #

<!--more-->

## Desicription ##

Write a function to find the longest common prefix string amongst an array of strings.

## Solution ##

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.size() == 0)
            return "";
        string res;
        for(int i = 0; strs[0][i]; i++){
            for(int j = 0; j < strs.size(); j++){
                if(strs[j][i] != strs[0][i] || strs[j].size() == i)
                    return res;
            }
            res += strs[0][i];
        }
        return res;
    }
};
```