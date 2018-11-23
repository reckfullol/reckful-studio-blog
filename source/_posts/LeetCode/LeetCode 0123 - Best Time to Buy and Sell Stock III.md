---
title: LeetCode 0123 - Best Time to Buy and Sell Stock III
date: 2017-12-18 15:09:35
categories: LeetCode
---
# Best Time to Buy and Sell Stock III #

<!--more-->

## Desicription ##

Say you have an array for which the *ith* element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

**Note:**
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

## Solution ##

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int hold1 = INT_MIN;
        int hold2 = INT_MIN;
        int release1 = 0;
        int release2 = 0;
        for(int price : prices) {
            hold1 = max(hold1, -price);
            release1 = max(release1, hold1 + price);
            hold2 = max(hold2, release1 - price);
            release2 = max(release2, hold2 + price);
        }
        return release2;
    }
};
```