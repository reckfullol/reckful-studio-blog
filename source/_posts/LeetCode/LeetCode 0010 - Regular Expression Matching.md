---
title: LeetCode 0010 - Regular Expression Matching
date: 2017-11-08 09:39:15
categories: LeetCode
---
# Regular Expression Matching #

<!--more-->

## Desicription ##

Implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

## Solution ##

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        if(p.empty())
            return s.empty();
        if(p[1] == '*')
            return isMatch(s, p.substr(2)) || (!s.empty() && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p));
        else
            return !s.empty() && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p.substr(1));
    }
};
```