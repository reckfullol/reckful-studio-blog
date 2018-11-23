---
title: LeetCode 0032 - Longest Valid Parentheses
date: 2017-11-11 14:59:06
categories: LeetCode
---
# Longest Valid Parentheses #

<!--more-->

## Desicription ##

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

For `"(()"`, the longest valid parentheses substring is `"()"`, which has length = 2.

Another example is `")()())"`, where the longest valid parentheses substring is `"()()"`, which has length = 4.

## Solution ##

```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int>Q;
        for(int i = 0; s[i]; i++){
            if(Q.empty())
                Q.push(i);
            else{
                if(s[Q.top()] == '(' && s[i] == ')')
                    Q.pop();
                else
                    Q.push(i);
            }
        }
        if(Q.empty())
            return s.length();
        int res = 0;
        int right = s.length();
        int left = 0;
        while(!Q.empty()){
            left = Q.top();
            Q.pop();
            res = max(res, right - left - 1);
            right = left;
        }
        res = max(res, right);
        return res;
    }
};
```