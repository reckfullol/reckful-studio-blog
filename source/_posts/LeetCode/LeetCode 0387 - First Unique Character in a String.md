---
title: LeetCode 0387 - First Unique Character in a String
date: 2019-08-15 22:26:38
categories: LeetCode
---
# First Unique Character in a String

<!--more-->

## Desicription

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

**Examples:**

```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

**Note:** You may assume the string contain only lowercase letters.

## Solution

```cpp
class Solution {
public:
    int firstUniqChar(const std::string& s) {
        auto book = std::vector<int>(26, false);
        for(int i = 0; i < s.size(); i++) {
            book[s[i] - 'a']++;
            }

        for(int i = 0; i < s.size(); i++) {
            if(book[s[i] - 'a'] == 1) {
                return i;
            }
        }

        return -1;
    }
};
```