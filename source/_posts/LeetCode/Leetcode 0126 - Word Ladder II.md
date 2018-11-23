---
title: LeetCode 0126 - Word Ladder II
date: 2018-05-24 14:42:17
categories: LeetCode
---
# Word Ladder II

<!--more-->

## Desicription

Given two words (*beginWord* and *endWord*), and a dictionary's word list, find all shortest transformation sequence(s) from *beginWord* to *endWord*, such that:

1. Only one letter can be changed at a time
2. Each transformed word must exist in the word list. Note that beginWord is not a transformed word.

**Note**:

- Return an empty list if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume beginWord and endWord are non-empty and are not the same.

**Example** 1:

```
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output:
    [
        ["hit","hot","dot","dog","cog"],
        ["hit","hot","lot","log","cog"]
    ]
```

**Example** 2:

```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: []

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

## Solution

```cpp
class Solution {
public:
    vector<vector<string>> findLadders(const string& beginWord, const string& endWord, vector<string>& wordList) {
        vector<vector<string>> res;
        unordered_set<string> dict(wordList.begin(), wordList.end());
        unordered_set<string> words;
        queue<vector<string>> paths;
        paths.push(vector<string>{beginWord});
        int step = 1;
        int minStep = INT_MAX;
        while(!paths.empty()) {
            vector<string> currentPath = paths.front();
            paths.pop();
            if(currentPath.size() > step) {
                for(const string &word : words)
                    dict.erase(word);
                words.clear();
                step = currentPath.size();
                if(step > minStep)
                    break;
            }
            string lastWord = currentPath.back();
            for(int i = 0; lastWord[i]; i++) {
                string tmpWord = lastWord;
                for(char ch = 'a'; ch <= 'z'; ch++) {
                    tmpWord[i] = ch;
                    if(!dict.count(tmpWord))
                        continue;
                    words.insert(tmpWord);
                    vector<string> nextPath = currentPath;
                    nextPath.push_back(tmpWord);
                    if(tmpWord == endWord)
                        res.push_back(nextPath), minStep = min(minStep, (int)nextPath.size());
                    else
                        paths.push(nextPath);
                }
            }
        }
        return res;
    }
};
```