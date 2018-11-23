---
title: LeetCode 0044 - Wildcard Matching
date: 2017-11-12 20:09:04
categories: LeetCode
---
# Wildcard Matching #

<!--more-->

## Desicription ##

Implement wildcard pattern matching with support for `'?'` and `'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```

## Solution ##

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int s_len = s.size();
        int p_len = p.size();
        int s_index = 0;
        int p_index = 0;
        int s_back_index = -1;
        int p_back_index = -1;
        for(s_index = 0, p_index = 0; s_index < s_len; s_index++, p_index++){
            if(p[p_index] == '*'){
                s_back_index = s_index;
                p_back_index = p_index;
                s_index--;
            }
            else{
                if(s[s_index] != p[p_index] && p[p_index] != '?'){
                    if(s_back_index >= 0){
                        s_index = s_back_index++;
                        p_index = p_back_index;
                    }
                    else
                        return 0;
                }
            }
        }
        while(p[p_index] == '*')
            p_index++;
        return p_index == p_len;
    }
};
```