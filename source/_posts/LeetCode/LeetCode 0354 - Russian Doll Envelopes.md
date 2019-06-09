---
title: LeetCode 0354 - Russian Doll Envelopes
date: 2019-06-09 16:00:16
categories: LeetCode
---
# Russian Doll Envelopes

<!--more-->

## Desicription

You have a number of envelopes with widths and heights given as a pair of integers (w, h). One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.

What is the maximum number of envelopes can you Russian doll? (put one inside other)

Note:

Rotation is not allowed.

**Example:**

```
Input: [[5,4],[6,4],[6,7],[2,3]]
Output: 3 
Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
```

## Solution

```cpp
class Solution {
public:
    int maxEnvelopes(std::vector<std::vector<int>>& envelopes) {
        std::sort(envelopes.begin(), envelopes.end(), [](std::vector<int> a, std::vector<int> b) {
            if(a[0] == b[0]) {
                return a[1] < b[1];
            }
            return a[0] < b[0];
        });

        int res = 0;

        auto dp = std::vector<int>(envelopes.size(), 0);
        for(int i = 0; i < dp.size(); i++) {
            dp[i] = 1;
            for(int j = 0; j < i; j++) {
                if(envelopes[j][0] < envelopes[i][0] && envelopes[j][1] < envelopes[i][1]) {
                    dp[i] = std::max(dp[i], dp[j] + 1);
                }
            }
            res = std::max(res, dp[i]);
        }

        return res;
    }
};
```