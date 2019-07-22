---
title: LeetCode 0383 - Ransom Note
date: 2019-07-22 11:05:38
categories: LeetCode
---
# Ransom Note

<!--more-->

## Desicription

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

**Note:**

You may assume that both strings contain only lowercase letters.

```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

## Solution

```cpp
class Solution {
public:
    bool canConstruct(std::string ransomNote, std::string magazine) {
        auto ransomCount = std::vector<int>(26, 0);
        auto magazineCount = std::vector<int>(26, 0);

        for(auto c : ransomNote) {
            ransomCount[c - 'a']++;
        }
        for(auto c : magazine) {
            magazineCount[c - 'a']++;
        }

        bool result = true;
        for(int i = 0; i < 26; i++) {
            if(ransomCount[i] > magazineCount[i]) {
                result = false;
                break;
            }
        }

        return result;
    }
};
```