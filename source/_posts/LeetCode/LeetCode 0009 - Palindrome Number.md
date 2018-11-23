---
title: LeetCode 0009 - Palindrome Number
date: 2017-11-08 09:37:46
categories: LeetCode
---
# Palindrome Number #

<!--more-->

## Desicription ##

Determine whether an integer is a palindrome. Do this without extra space.

## Solution ##

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        stringstream stream;
        string res;
        stream << x;
        stream >> res;
        for(int i = 0, j = res.size()-1; i <= j; i++, j--){
            if(res[i] != res[j])
                return 0;
        }        
        return 1;
    }
};
```