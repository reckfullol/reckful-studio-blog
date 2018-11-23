---
title: LeetCode 0125 - Valid Palindrome
date: 2017-12-18 15:27:29
categories: LeetCode
---
# Valid Palindrome

<!--more-->

## Desicription

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
`"A man, a plan, a canal: Panama"` is a palindrome.
`"race a car"` is *not* a palindrome.

**Note:**
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.

## Solution

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        string res = "";
        for(auto it : s) {
            if((it >= 'a' && it <= 'z') || (it >= '0' && it <= '9'))
                res += it;
            else if(it >= 'A' && it <= 'Z')
                res += it + 'a' - 'A';
        }
        for(int i = 0, j = res.size()-1; i <= j; i++, j--) {
            if(res[i] != res[j])
                return 0;
        }
        return 1;
    }
};
```