---
title: LeetCode 0043 - Multiply Strings
date: 2017-11-11 21:34:58
categories: LeetCode
---
# Multiply Strings #

<!--more-->

## Desicription ##

Given two non-negative integers ``num1`` and ``num2`` represented as strings, return the product of ``num1`` and ``num2``.

**Note:**

1. The length of both `num1` and `num2` is < 110.
1. Both `num1` and `num2` contains only digits 0-9.
1. Both `num1` and `num2` does not contain any leading zero.
1. You **must not use any built-in BigInteger library or convert the inputs to integer** directly.

## Solution ##

```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        string res(num1.size() + num2.size(), '0');
        for(int i = num1.size()-1; i >= 0; i--){
            int carry = 0;
            for(int j = num2.size()-1; j >= 0; j--){
                int tmp = res[i+j+1] - '0' + (num1[i] - '0') * (num2[j] - '0') + carry;
                res[i+j+1] = tmp % 10 + '0';
                carry = tmp / 10;
            }
            res[i] += carry;
        }
        size_t index = res.find_first_not_of("0");
        if(index != string::npos)
            return res.substr(index);
        return "0";
    }
};
```