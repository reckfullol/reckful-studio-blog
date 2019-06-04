---
title: LeetCode 0336 - Palindrome Pairs
date: 2019-06-04 11:45:45
categories: LeetCode
---
# Palindrome Pairs

<!--more-->

## Desicription

Given a list of unique words, find all pairs of distinct indices (i, j) in the given list, so that the concatenation of the two words, i.e. words[i] + words[j] is a palindrome.

**Example 1:**

```
Input: ["abcd","dcba","lls","s","sssll"]
Output: [[0,1],[1,0],[3,2],[2,4]] 
Explanation: The palindromes are ["dcbaabcd","abcddcba","slls","llssssll"]
```

**Example 2:**

```
Input: ["bat","tab","cat"]
Output: [[0,1],[1,0]] 
Explanation: The palindromes are ["battab","tabbat"]
```

## Solution

```cpp
class Solution {
public:
    std::vector<std::vector<int>> palindromePairs(std::vector<std::string>& words) {
        auto res = std::vector<std::vector<int>>();
        auto indexMap = std::map<std::string, int>();
        auto sizeSet = std::set<int>();

        for(int i = 0; i < words.size(); i++) {
            indexMap.insert({words[i], i});
            sizeSet.insert(words[i].size());
        }

        for(int i = 0; i < words.size(); i++) {
            auto word = words[i];
            std::reverse(word.begin(), word.end());

            if(indexMap.find(word) != indexMap.end() && indexMap[word] != i) {
                res.push_back({i, indexMap[word]});
            }

            auto rightIterator = sizeSet.find(word.size());
            for(auto it = sizeSet.begin(); it != rightIterator; it++) {
                if(isPalindrome(word, 0, word.size() - *it - 1) && indexMap.find(word.substr(word.size() - *it)) != indexMap.end()) {
                    res.push_back({i, indexMap[word.substr(word.size() - *it)]});
                }

                if(isPalindrome(word, *it, word.size() - 1) && indexMap.find(word.substr(0, *it)) != indexMap.end()) {
                    res.push_back({indexMap[word.substr(0, *it)], i});
                }
            }
        }

        return res;
    }

private:
    static bool isPalindrome(const std::string &word, int left, int right) {
        while(left < right) {
            if(word[left++] != word[right--]) {
                return false;
            }
        }
        return true;
    }
};
```