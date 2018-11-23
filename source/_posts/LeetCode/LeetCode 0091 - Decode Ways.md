---
title: LeetCode 0091 - Decode Ways
date: 2017-12-15 10:46:54
categories: LeetCode
---
# Decode Ways #

<!--more-->

## Desicription ##

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message `"12"`, it could be decoded as `"AB"` (1 2) or `"L"` (12).

The number of ways decoding `"12"` is 2.

## Solution ##

```cpp
class Solution {
public:
    int numDecodings(string s) {
        if(s.empty() || s[0] == '0')
            return 0;
        int r1 = 1;
        int r2 = 1;
        for(int i = 1; s[i]; i++) {
            if(s[i] == '0')
                r1 = 0;
            if(s[i-1] == '1' || (s[i-1] == '2' && s[i] <= '6'))
                r1 += r2, r2 = r1 - r2;
            else
                r2 = r1;
        }
        return r1;
    }
};
```