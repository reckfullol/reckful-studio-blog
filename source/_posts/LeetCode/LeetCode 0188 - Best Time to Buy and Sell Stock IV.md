---
title: LeetCode 0188 - Best Time to Buy and Sell Stock IV
date: 2018-06-06 21:06:09
categories: LeetCode
---
# Best Time to Buy and Sell Stock IV

<!--more-->

## Desicription

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most k transactions.

**Note**:
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

**Example** 1:

```
Input: [2,4,1], k = 2
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```

**Example** 2:

```
Input: [3,2,6,5,0,3], k = 2
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4.
             Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
```

## Solution

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        priority_queue<int> profits;
        stack<pair<int, int>> vps;
        int v = 0;
        int p = 0;
        int len = prices.size();
        while(p < len) {
            for(v = p; v < len-1 && prices[v] >= prices[v+1]; v++);
            for(p = v+1; p < len && prices[p] >= prices[p-1]; p++);
            while(vps.size() && prices[vps.top().first] > prices[v]) {
                profits.push(prices[vps.top().second - 1] - prices[vps.top().first]);
                vps.pop();
            }
            while(vps.size() && prices[vps.top().second-1] <= prices[p-1]) {
                profits.push(prices[vps.top().second-1] - prices[v]);
                v = vps.top().first;
                vps.pop();
            }
            vps.push(pair<int, int>(v, p));
        }
        while(vps.size()) {
            profits.push(prices[vps.top().second-1] - prices[vps.top().first]);
            vps.pop();
        }
        int res = 0;
        while(k-- && profits.size())
            res += profits.top(), profits.pop();
        return res;
    }
};
```