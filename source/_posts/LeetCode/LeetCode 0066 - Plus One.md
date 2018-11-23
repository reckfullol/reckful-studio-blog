---
title: LeetCode 0066 - Plus One
date: 2017-11-17 16:20:09
categories: LeetCode
---
# Plus One #

<!--more-->

## Desicription ##

Given a non-negative integer represented as a **non-empty** array of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.

## Solution ##

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        digits[digits.size()-1]++;
        for(int i = digits.size()-1; i > 0; i--)
            if(digits[i] >= 10)
                digits[i] -= 10, digits[i-1]++;
        if(digits[0] >= 10)
            digits[0] -= 10, digits.insert(digits.begin(), 1);
        return digits;
    }
};
```