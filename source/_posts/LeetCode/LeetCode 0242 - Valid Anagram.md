---
title: LeetCode 0242 - Valid Anagram
date: 2018-12-09 14:38:26
categories: LeetCode
---
# Valid Anagram

<!--more-->

## Desicription

Given two strings s and t , write a function to determine if t is an anagram of s.

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

**Note:**

You may assume the string contains only lowercase alphabets.

## Solution

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        vector<int> s_cnt(26, 0);
        vector<int> t_cnt(26, 0);
        for(const auto& a : s) {
            s_cnt[a - 'a']++;
        }
        for(const auto& a : t) {
            t_cnt[a - 'a']++;
        }
        return s_cnt == t_cnt;
    }
};
```