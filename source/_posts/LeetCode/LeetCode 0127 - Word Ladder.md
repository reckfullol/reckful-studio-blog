---
title: LeetCode 0127 - Word Ladder
date: 2018-05-24 14:46:08
categories: LeetCode
---
# Word Ladder

<!--more-->

## Desicription

Given two words (*beginWord* and *endWord*), and a dictionary's word list, find the length of shortest transformation sequence from *beginWord* to *endWord*, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list. Note that beginWord is not a transformed word.

**Note**:

- Return 0 if there is no such transformation sequence.
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

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

**Example** 2:

```
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

## Solution

```cpp
class Solution {
private:
    bool isNeighbor(string& a, string& b) {
        int cnt = 0;
        for(int i = 0; a[i]; i++) {
            if(a[i] != b[i])
                cnt++;
        }
        return cnt == 1;
    }
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        queue<string> cur;
        queue<string> next;
        cur.push(beginWord);
        int step = 2;
        while(cur.size()) {
            string tmp = cur.front();
            cur.pop();
            for(auto& it : wordList) {
                if(it == "" || !isNeighbor(it, tmp))
                    continue;
                if(it == endWord)
                    return step;
                next.push(it);
                it = "";
            }
            if(cur.empty()){
                step++;
                swap(cur, next);
            }
        }    
        return 0;
    }
};
```