---
title: LeetCode 0166 - Fraction to Recurring Decimal
date: 2018-06-04 09:56:28
categories: LeetCode
---
# Fraction to Recurring Decimal

<!--more-->

## Desicription

Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.

If the fractional part is repeating, enclose the repeating part in parentheses.

**Example** 1:

```
Input: numerator = 1, denominator = 2
Output: "0.5"
```

**Example** 2:

```
Input: numerator = 2, denominator = 1
Output: "2"
```

**Example** 3:

```
Input: numerator = 2, denominator = 3
Output: "0.(6)"
```

## Solution

```cpp
class Solution {
public:
    string fractionToDecimal(long long numerator, long long denominator) {
        if(numerator == 0)
            return "0";
        string res;
        if(numerator < 0 ^ denominator < 0)
            res += "-";
        numerator = abs(numerator);
        denominator = abs(denominator);
        res += to_string(numerator / denominator);
        if(numerator % denominator == 0)
            return res;
        res += ".";
        unordered_map<long long, long long> mp;
        for(long long r = numerator % denominator; r; r %= denominator) {
            if(mp.count(r)) {
                res.insert(mp[r], 1, '(');
                res += ")";
                return res;
            }
            mp[r] = res.size();
            r *= 10;
            res += to_string(r / denominator);
        }
        return res;
    }
};
```