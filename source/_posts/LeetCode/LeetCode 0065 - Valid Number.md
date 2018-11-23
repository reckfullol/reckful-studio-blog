---
title: LeetCode 0065 - Valid Number
date: 2017-11-18 20:40:19
categories: LeetCode
---
# Valid Number #

<!--more-->

## Desicription ##

Validate if a given string is numeric.

Some examples:

`"0"` => `true`

`" 0.1 "` => `true`

`"abc"` => `false`

`"1 a"` => `false`

`"2e10"` => `true`

**Note:** It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one.

## Solution ##

```cpp
class Solution {
public:
    bool isNumber(string s) {
        int index = 0;
        for(; s[index] == ' '; index++);
        if(s[index] == '+' || s[index] == '-')
            index++;
        int n_num = 0, n_pt = 0;
        for(; ((s[index] <= '9' && s[index] >= '0') || s[index] == '.'); index++)
            s[index] == '.'?n_pt++:n_num++;
        if(!n_num || n_pt > 1)
            return 0;
        if(s[index] == 'e'){
            index++;
            if(s[index] == '+' || s[index] == '-')
                index++;
            int n_num = 0;
            for(; s[index] >= '0' && s[index] <= '9'; index++)
                n_num++;
            if(!n_num)
                return 0;
        }
        for(; s[index] == ' '; index++);
        return !s[index];
    }
};
```