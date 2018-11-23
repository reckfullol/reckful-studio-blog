---
title: LeetCode 0076 - Minimum Window Substring
date: 2017-11-19 14:13:28
categories: LeetCode
---
# Minimum Window Substring #

<!--more-->

## Desicription ##

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

For example,

**S** = `"ADOBECODEBANC"`

**T** = `"ABC"`

Minimum window is `"BANC"`.

Note:
If there is no such window in S that covers all characters in T, return the empty string `""`.

If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.

## Solution ##

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        if(s.empty() || t.empty())
            return "";
        int cnt = t.size();
        vector<int> require(128, 0);
        vector<bool> book(128, 0);
        for(int i = 0; t[i]; ++i)
            require[t[i]]++, book[t[i]] = 1;
        int left = 0;
        int right = -1;
        int min_len = INT_MAX;
        int min_index = 0;
        while(left < s.size() && right < (int)s.size()){
            if(cnt){
                require[s[++right]]--;
                if(book[s[right]] && require[s[right]] >= 0)
                    cnt--;
            }
            else{
                if(min_len > right - left + 1)
                    min_len = right - left + 1, min_index = left;
                require[s[left]]++;
                if(book[s[left]] && require[s[left]] > 0)
                    cnt++;
                left++;
            }
        }
        if(min_len == INT_MAX)
            return "";
        return s.substr(min_index, min_len);
    }
};
```