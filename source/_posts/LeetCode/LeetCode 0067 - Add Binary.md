---
title: LeetCode 0067 - Add Binary
date: 2017-11-17 16:30:56
categories: LeetCode
---
# Add Binary #

<!--more-->

## Desicription ##

Given two binary strings, return their sum (also a binary string).

For example,

a = `"11"`

b = `"1"`

Return `"100"`.

## Solution ##

```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        string res;
        int a_index = a.size() - 1;
        int b_index = b.size() - 1;
        int carry = 0;
        while(a_index >= 0 || b_index >= 0 || carry){
            carry += a_index >= 0? a[a_index--] - '0' : 0;
            carry += b_index >= 0? b[b_index--] - '0' : 0;
            res = char (carry % 2 + '0') + res;
            carry >>= 1; 
        }
        return res;    
    }
};
```