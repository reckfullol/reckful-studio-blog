---
title: LeetCode 0318 - Maximum Product of Word Lengths
date: 2019-05-17 12:13:04
categories: LeetCode
---
# Maximum Product of Word Lengths

<!--more-->

## Desicription

Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

**Example 1:**

```
Input: ["abcw","baz","foo","bar","xtfn","abcdef"]
Output: 16 
Explanation: The two words can be "abcw", "xtfn".
```

**Example 2:**

```
Input: ["a","ab","abc","d","cd","bcd","abcd"]
Output: 4 
Explanation: The two words can be "ab", "cd".
```

**Example 3:**

```
Input: ["a","aa","aaa","aaaa"]
Output: 0 
Explanation: No such pair of words.
```

## Solution

```cpp
class Solution {
public:
    int maxProduct(std::vector<std::string>& words) {
        auto visits = std::vector<std::bitset<26>>(words.size(), 0);
        for(int i = 0; i < words.size(); i++) {
            auto word = words[i];
            for(char c : word) {
                visits[i][c - 'a'] = true;
            }
        }

        int maxLength = 0;
        for(int i = 0; i < words.size(); i++) {
            for(int j = i + 1; j < words.size(); j++) {
                if((visits[i] & visits[j]).to_ulong() == 0) {
                    maxLength = std::max(maxLength, (int)words[i].size() * (int)words[j].size());
                }
            }
        }

        return maxLength;
    }
};
```