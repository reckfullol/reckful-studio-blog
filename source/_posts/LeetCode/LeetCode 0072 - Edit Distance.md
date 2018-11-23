---
title: LeetCode 0072 - Edit Distance
date: 2017-11-18 21:36:56
categories: LeetCode
---
# Edit Distance #

<!--more-->

## Desicription ##

Given two words *word1* and *word2*, find the minimum number of steps required to convert *word1* to *word2*. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

a) Insert a character

b) Delete a character

c) Replace a character

## Solution ##

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size();
        int m = word2.size();
        vector<vector<int>> dp(n+1, vector<int>(m+1, 0));
        for(int i = 1; i <= n; i++)
            dp[i][0] = i;
        for(int i = 1; i <= m; i++)
            dp[0][i] = i;
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                if(word1[i-1] == word2[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else
                    dp[i][j] = min(dp[i-1][j-1] + 1, min(dp[i-1][j] + 1, dp[i][j-1] + 1));
            }
        }
        return dp[n][m];
    }
};
```