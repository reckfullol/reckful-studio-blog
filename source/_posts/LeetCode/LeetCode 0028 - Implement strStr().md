---
title: LeetCode 0028 - Implement strStr()
date: 2017-11-10 19:37:38
categories: LeetCode
---
# Implement strStr() #

<!--more-->

## Desicription ##

Implement [strStr()](http://www.cplusplus.com/reference/cstring/strstr/).

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

## Solution ##

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        return haystack.find(needle);
    }
};
```