---
title: LeetCode 0013 - Roman to Integer
date: 2017-11-09 19:47:45
categories: LeetCode
---
# Roman to Integer #

<!--more-->

## Desicription ##

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

## Solution ##

```cpp
class Solution {
private:
    unordered_map<char, int> mp = {
        {'I', 1},
        {'V', 5},
        {'X', 10},
        {'L', 50},
        {'C', 100},
        {'D', 500},
        {'M', 1000}
    };

public:
    int romanToInt(string s) {
        int res = mp[s[0]];
        for(int i = 1; s[i]; i++){
            if(mp[s[i]] > mp[s[i-1]])
                res += mp[s[i]] - mp[s[i-1]] * 2;
            else
                res += mp[s[i]];
        }        
        return res;
    }
};
```