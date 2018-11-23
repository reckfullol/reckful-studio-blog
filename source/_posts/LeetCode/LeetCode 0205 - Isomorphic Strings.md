---
title: LeetCode 0205 - Isomorphic Strings
date: 2018-06-12 12:21:06
categories: LeetCode
---
# Isomorphic Strings

<!--more-->

## Desicription

Given two strings **s** and **t**, determine if they are isomorphic.

Two strings are isomorphic if the characters in **s** can be replaced to get **t**.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

**Example** 1:

```
Input: s = "egg", t = "add"
Output: true
```

**Example** 2:

```
Input: s = "foo", t = "bar"
Output: false
```

**Example** 3:

```
Input: s = "paper", t = "title"
Output: true
```

**Note**:
You may assume both **s** and **t** have the same length.

## Solution

```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        vector<int> sIndex(256, 0);
        vector<int> tIndex(256, 0);
        for(int i = 0; s[i]; i++) {
            if(sIndex[s[i]] != tIndex[t[i]])
                return false;
            sIndex[s[i]] = i + 1;
            tIndex[t[i]] = i + 1;
        }
        return true;
    }
};
```