---
title: LeetCode 0357 - Count Numbers with Unique Digits
date: 2019-06-12 16:13:44
categories: LeetCode
---
# Count Numbers with Unique Digits

<!--more-->

## Desicription

Given a non-negative integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

**Example:**

```
Input: 2
Output: 91 
Explanation: The answer should be the total numbers in the range of 0 ≤ x < 100, 
             excluding 11,22,33,44,55,66,77,88,99
```

## Solution

```cpp
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        if(n == 0) {
            return 1;
        }

        int res = 10;
        int count = 9;
        for(int i = 2; i <= n; i++) {
            count *= (11 - i);
            res += count;
        }

        return res;
    }
};
```