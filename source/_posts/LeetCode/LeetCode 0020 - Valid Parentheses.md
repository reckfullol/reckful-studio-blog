---
title: LeetCode 0020 - Valid Parentheses
date: 2017-11-09 20:15:37
categories: LeetCode
---
# Valid Parentheses #

<!--more-->

## Desicription ##

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

## Solution ##

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> res;
        for(int i = 0; s[i]; i++){
            if(s[i] == '(' || s[i] == '[' || s[i] == '{')
                res.push(s[i]);
            else if(s[i] == ')'){
                if(res.empty() || res.top() != '(')
                    return 0;
                else
                    res.pop();
            }
            else if(s[i] == ']'){
                if(res.empty() || res.top() != '[')
                    return 0;
                else
                    res.pop();
            }
            else if(s[i] == '}'){
                if(res.empty() || res.top() != '{')
                    return 0;
                else
                    res.pop();
            }
        }
        return res.empty();
    }
};
```