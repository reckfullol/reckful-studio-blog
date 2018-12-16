---
title: LeetCode 0279 - Perfect Squares
date: 2018-12-16 15:40:47
categories: LeetCode
---
# Perfect Squares

<!--more-->

## Desicription

Given a positive integer *n*, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to *n*.

**Example 1:**

```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.
```

**Example 2:**

```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

## Solution

```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1, INT_MAX);
        dp[0] = 0;
        for(int i = 1, squares; (squares = i * i) <= n; i++) {
            for(int j = squares; j <= n; j++) {
                if(dp[j] > dp[j - squares] + 1) {
                    dp[j] = dp[j - squares] + 1;
                }
            }
        }
        return dp[n];
    }
};
```