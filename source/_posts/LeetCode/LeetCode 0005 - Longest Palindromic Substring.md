---
title: LeetCode 0005 - Longest Palindromic Substring
date: 2017-11-08 08:50:53
categories: LeetCode
---
# Longest Palindromic Substring #

<!--more-->

## Desicription ##

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example:**

```
Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
```

**Example:**

```
Input: "cbbd"

Output: "bb"
```

## Solution ##

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        s.insert(0, "$");
        int maxn = 1;
        int left = 0;
        int right = 0;
        for(int i = 0; s[i]; i++){
            int left_tmp = i, right_tmp = i;
            while(s[right_tmp+1] == s[i])
                right_tmp++;
            i = right_tmp;
            while(s[left_tmp-1] == s[right_tmp+1])
                left_tmp--, right_tmp++;
                if(right_tmp - left_tmp + 1 > maxn)
                    maxn = right_tmp - left_tmp + 1, right = right_tmp, left = left_tmp;
        }
        if(!left && !right)
            return s.substr(1,1);
        return s.substr(left, right - left + 1);
    }
};
```