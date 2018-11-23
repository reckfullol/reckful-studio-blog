---
title: LeetCode 0211 - Add and Search Word - Data structure design
date: 2018-06-12 17:11:39
categories: LeetCode
---
# Add and Search Word - Data structure design

<!--more-->

## Desicription

Design a data structure that supports the following two operations:

```
void addWord(word)
bool search(word)
```

search(word) can search a literal word or a regular expression string containing only letters `a-z` or `.`. A `.` means it can represent any one letter.

**Example**:

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

**Note**:

You may assume that all words are consist of lowercase letters `a-z`.

## Solution

```cpp
/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * bool param_2 = obj.search(word);
 */
class WordDictionary {
private:
    class TrieNode {
    public:
        bool isWord = false;
        TrieNode* children[26] = {nullptr};
    };
    TrieNode* root = new TrieNode();
public:
    /** Initialize your data structure here. */
    WordDictionary() = default;

    /** Adds a word into the data structure. */
    void addWord(string word) {
        TrieNode* run = root;
        for(char c : word) {
            if(nullptr == run->children[c - 'a'])
                run->children[c - 'a'] = new TrieNode();
            run = run->children[c - 'a'];
        }
        run->isWord = true;
    }

    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool search(string word) {
        return query(std::move(word), root);
    }
    bool query(string word, TrieNode* run, int index = 0) {
        for(int i = index; word[i]; i++) {
            if(nullptr != run && word[i] != '.')
                run = run->children[word[i] - 'a'];
            else if(nullptr != run && word[i] == '.') {
                TrieNode* tmp = run;
                for(char c = 'a'; c <= 'z'; c++) {
                    word[i] = c;
                    run = tmp->children[word[i] - 'a'];
                    if(query(word, run, i+1))
                        return true;
                }
            }
            else break;
        }
        return run && run->isWord;
    }
};
```
