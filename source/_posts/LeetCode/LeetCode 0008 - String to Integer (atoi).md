---
title: LeetCode 0008 - String to Integer (atoi)
date: 2017-11-08 08:59:35
categories: LeetCode
---
# String to Integer (atoi) #

<!--more-->

## Desicription ##

Implement atoi to convert a string to an integer.

**Hint:** Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

**Notes:** It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

## Solution ##

```cpp
class Solution {
public:
    int myAtoi(string str) {
        if(str == "")
            return 0;
        stringstream ssTmp(str);
        int res;
        ssTmp >> res;
        return res;
    }
};
```