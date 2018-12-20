---
title: LeetCode 0290 - Word Pattern
date: 2018-12-20 09:49:51
categories: LeetCode
---
# Word Pattern

<!--more-->

## Desicription

Given a **pattern** and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a **non-empty** word in str.

**Example 1:**

```
Input: pattern = "abba", str = "dog cat cat dog"
Output: true
```

**Example 2:**

```
Input:pattern = "abba", str = "dog cat cat fish"
Output: false
```

**Example 3:**

```
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false
```

**Example 4:**

```
Input: pattern = "abba", str = "dog dog dog dog"
Output: false
```

**Notes:**
You may assume `pattern` contains only lowercase letters, and `str` contains lowercase letters separated by a single space.

## Solution

```cpp
class Solution {
public:
    bool wordPattern(string pattern, string str) {
        vector<string> vec_str;
        istringstream is(str);
        for(string s; is >> s; ) {
            vec_str.push_back(s);
        }
        if(pattern.size() != vec_str.size()) {
            return false;
        }
        map<char, int> pattern_map;
        map<string, int> string_map;
        for(int i = 0; i < pattern.size(); i++) {
            if(pattern_map[pattern[i]] != string_map[vec_str[i]]) {
                return false;
            }
            pattern_map[pattern[i]] = i + 1;
            string_map[vec_str[i]] = i + 1;
        }
        return true;
    }
};
```