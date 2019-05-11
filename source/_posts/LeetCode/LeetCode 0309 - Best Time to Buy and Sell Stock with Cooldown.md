---
title: LeetCode 0309 - Best Time to Buy and Sell Stock with Cooldown
date: 2019-05-11 17:07:47
categories: LeetCode
---
# Best Time to Buy and Sell Stock with Cooldown

<!--more-->

## Desicription

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

- You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
- After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

**Example:**

```

Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]

```

## Solution

```cpp
class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        auto dp = std::vector<std::vector<int>>(2, std::vector<int>(prices.size() + 1, INT_MIN));
        dp[0][0] = 0;
        for(int i = 1; i < dp[0].size(); i++) {
            if(i == 1) {
                dp[0][1] = 0;
                dp[1][1] = -prices[0];
            } else {
                dp[0][i] = std::max(dp[0][i-1], dp[1][i-1] + prices[i-1]);
                dp[1][i] = std::max(dp[1][i-1], dp[0][i-2] - prices[i-1]);
            }
        }
        return dp[0][dp[0].size() -1];
    }
};
```
