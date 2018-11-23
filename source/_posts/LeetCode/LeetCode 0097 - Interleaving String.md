---
title: LeetCode 0097 - Interleaving String
date: 2017-12-15 14:29:11
categories: LeetCode
---
# Interleaving String #

<!--more-->

## Desicription ##

Given *s1*, *s2*, *s3*, find whether *s3* is formed by the interleaving of *s1* and *s2*.

For example,
Given:
*s1* = `"aabcc"`,
*s2* = `"dbbca"`,

When *s3* = `"aadbbcbcac"`, return true.
When *s3* = `"aadbbbaccc"`, return false.

## Solution ##

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        if(s1.size() + s2.size() != s3.size())
            return 0;
        vector<vector<bool>> table(s1.size()+1, vector<bool>(s2.size()+1, 0));
        for(int i = 0; i <= s1.size(); i++) {
            for(int j = 0; j <= s2.size(); j++){
                if(!i && !j)
                    table[i][j] = 1;
                else if(!i)
                    table[i][j] = (table[i][j-1] && s2[j-1] == s3[i+j-1]);
                else if(!j)
                    table[i][j] = (table[i-1][j] && s1[i-1] == s3[i+j-1]);
                else
                    table[i][j] = ((table[i-1][j] && s1[i-1] == s3[i+j-1]) || (table[i][j-1] && s2[j-1] == s3[i+j-1]));
            }
        }
        return table[s1.size()][s2.size()];
    }
};
```