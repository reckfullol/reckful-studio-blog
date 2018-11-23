---
title: Sword To Offer 052 - 正则表达式匹配
date: 2018-05-12 16:00:22
categories: Sword To Offer
---
# 正则表达式匹配

<!--more-->

## Desicription

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

## Solution

```cpp
class Solution {
public:
    bool match(char str[], char pattern[]) {

        if(str[0] == '\0' && pattern[0] == '\0') {
            return true;
        }

        if(pattern[0] != '\0' && pattern[1] == '*') {
            if(match(str, pattern + 2)) {
                return true;
            }
        }

        if((str[0] != '\0' && pattern[0] == '.') || (str[0] == pattern[0])) {
            if(match(str + 1, pattern + 1)) {
                return true;
            }
            if(pattern[1] == '*') {
                if(match(str + 1, pattern)) {
                    return true;
                }
            }
        }

        return false;
    }
};
```