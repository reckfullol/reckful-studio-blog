---
title: LeetCode 0003 - Longest Substring Without Repeating Characters
date: 2017-11-07 21:16:58
categories: LeetCode
---
# Longest Substring Without Repeating Characters #

<!--more-->

## Desicription ##

Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.

Given `"bbbbb"`, the answer is `"b"`, with the length of 1.

Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a **substring**, `"pwke"` is a subsequence and not a substring.

## Solution ##

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s.empty())
            return 0;
        int res = 1;
        int left = 0;
        int right = 0;
        bool book[200];
        memset(book, 0, sizeof(book));
        for(right = 0; s[right]; right++){
            while(book[(int) s[right]] && left != right){
                book[(int) s[left]] = 0;
                left++;
            }
            book[(int) s[right]] = 1;
            res = max(right - left + 1, res);
        }
        return res;
    }
};
```