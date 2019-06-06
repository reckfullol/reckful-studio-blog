---
title: LeetCode 0345 - Reverse Vowels of a String
date: 2019-06-06 21:09:09
categories: LeetCode
---
# Reverse Vowels of a String

<!--more-->

## Desicription

Write a function that takes a string as input and reverse only the vowels of a string.

**Example 1:**

```
Input: "hello"
Output: "holle"
```

**Example 2:**

```
Input: "leetcode"
Output: "leotcede"
```

Note:

The vowels does not include the letter "y".

## Solution

```cpp
class Solution {
public:
    std::string reverseVowels(const std::string& s) {
        auto vowels = std::string();
        for(auto c : s) {
            if(c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' || c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U') {
                vowels += c;
            }
        }
        std::reverse(vowels.begin(), vowels.end());

        auto res = std::string();
        int index = 0;
        for(auto c : s) {
            if(c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' || c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U') {
                res += vowels[index++];
            } else {
                res += c;
            }
        }

        return res;
    }
};
```