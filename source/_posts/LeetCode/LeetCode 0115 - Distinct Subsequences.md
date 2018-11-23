---
title: LeetCode 0115 - Distinct Subsequences
date: 2017-12-18 12:53:15
categories: LeetCode
---
# Distinct Subsequences #

<!--more-->

## Desicription ##

Given a string **S** and a string **T**, count the number of distinct subsequences of **S** which equals **T**.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not).

Here is an example:
**S** = `"rabbbit"`, **T** = `"rabbit"`

Return `3`.

## Solution ##

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<int>> dp(t.size() + 1, vector<int>(s.size() + 1, 0));
        for(int i = 0; i <= s.size(); i++)
            dp[0][i] = 1;
        for(int i = 1; i <= t.size(); i++) {
            for(int j = 1; j <= s.size(); j++) {
                if(t[i-1] == s[j-1])
                    dp[i][j] = dp[i-1][j-1] + dp[i][j-1];
                else
                    dp[i][j] = dp[i][j-1];
            }
        }
        return dp[t.size()][s.size()];
    }
};
```