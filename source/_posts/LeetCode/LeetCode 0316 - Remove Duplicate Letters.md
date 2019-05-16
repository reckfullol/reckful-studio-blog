---
title: LeetCode 0316 - Remove Duplicate Letters
date: 2019-05-16 20:48:01
categories: LeetCode
---
# Remove Duplicate Letters

<!--more-->

## Desicription

Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

**Example 1:**

```
Input: "bcabc"
Output: "abc"
```

**Example 2:**

```
Input: "cbacdcbc"
Output: "acdb"
```

## Solution

```cpp
class Solution {
public:
    std::string removeDuplicateLetters(std::string s) {
        auto visit = std::map<char, bool>();
        auto count = std::map<char, int>();

        for(auto c : s) {
            count[c]++;
        }

        auto res = std::string();
        for(auto c : s) {
            count[c]--;

            if(visit[c]) {
                continue;
            }

            while(!res.empty() && c < res.back() && count[res.back()] != 0) {
                visit[res.back()] = false;
                res.pop_back();
            }

            res += c;
            visit[c] = true;
        }

        return res;
    }
};
```
