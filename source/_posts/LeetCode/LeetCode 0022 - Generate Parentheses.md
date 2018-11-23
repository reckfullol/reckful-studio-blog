---
title: LeetCode 0022 - Generate Parentheses
date: 2017-11-10 18:51:25
categories: LeetCode
---
# Generate Parentheses #

<!--more-->

## Desicription ##

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

## Solution ##

```cpp
class Solution {
public:
    vector<string> res;
    vector<string> generateParenthesis(int n) {
        addPair("", n, 0);
        return res;
    }
private:
    void addPair(string str, int n, int m){
        if(!n && !m){
            res.push_back(str);
            return ;
        }
        if(m)
            addPair(str+')', n, m-1);
        if(n)
            addPair(str+'(', n-1, m+1);
    }
};
```