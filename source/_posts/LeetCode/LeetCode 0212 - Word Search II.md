---
title: LeetCode 0212 - Word Search II
date: 2018-06-19 09:17:25
categories: LeetCode
---
# Word Search II

<!--more-->

## Desicription

Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example**:

```
Input: 
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

Output: ["eat","oath"]
```

**Note**:

You may assume that all inputs are consist of lowercase letters `a-z`.

## Solution

```cpp
class Solution {
private:
    vector<vector<int>> dir{{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    struct TrieNode{
        char value;
        vector<TrieNode*> children;

        explicit TrieNode(char c):value(c){};
    };
    TrieNode* root = new TrieNode('#');
    vector<string> result;

    void dfs(vector<vector<char>>& board, int x, int y, string currentString, TrieNode* currentNode) {
        for(auto& childrenNode : currentNode->children) {
            if(board[x][y] == childrenNode->value) {
                currentString += board[x][y];
                for(auto& nextNode : childrenNode->children) {
                    if(nextNode->value == '#') {
                        result.push_back(currentString);
                        childrenNode->children.erase(find(childrenNode->children.begin(), childrenNode->children.end(), nextNode));
                    }
                }
                board[x][y] ^= 255;
                for(int i = 0; i < 4; i++) {
                    int dx = x + dir[i][0];
                    int dy = y + dir[i][1];
                    if(dx < 0 || dx >= board.size() || dy < 0 || dy >= board[0].size()) {
                        continue;
                    }
                    dfs(board, dx, dy, currentString, childrenNode);
                }
                board[x][y] ^= 255;
            }
        }
    }
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        set<string> wordsSet;
        for(const auto &currentWord : words) {
            wordsSet.insert(currentWord);
        }
        for(auto currentWord : wordsSet) {
            if(currentWord.empty()) {
                result.emplace_back("");
                continue;
            }
            currentWord += "#";
            int index = 0;
            auto pre = root;
            while(currentWord[index]) {
                bool found = false;
                for(auto children : pre->children) {
                    if(children->value == currentWord[index]) {
                        found = true;
                        pre = children;
                        break;
                    }
                }
                if(!found) {
                    auto nextNode = new TrieNode(currentWord[index]);
                    pre->children.push_back(nextNode);
                    pre = nextNode;
                }
                index++;
            }
        }
        if(board.empty() || board[0].empty()) {
            return result;
        }
        for(int i = 0; i < board.size(); i++) {
            for(int j = 0; j < board[0].size(); j++) {
                dfs(board, i, j, "", root);
            }
        }
        return result;
    }
};
```