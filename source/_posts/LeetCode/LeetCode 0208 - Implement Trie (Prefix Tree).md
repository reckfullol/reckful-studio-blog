---
title: LeetCode 0208 - Implement Trie (Prefix Tree)
date: 2018-06-12 16:53:13
categories: LeetCode
---
# Implement Trie (Prefix Tree)

<!--more-->

## Desicription

Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example**:

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

**Note**:

- You may assume that all inputs are consist of lowercase letters a-z.
- All inputs are guaranteed to be non-empty strings.

## Solution

```cpp
/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * bool param_2 = obj.search(word);
 * bool param_3 = obj.startsWith(prefix);
 */
class Trie {
private:
    struct TrieNode {
        TrieNode* next[26] = {NULL};
        bool isWord = false;
    };
    TrieNode* find(string word) {
        TrieNode* p = root;
        for(int i = 0; word[i] && p; i++) {
            p = p->next[word[i]-'a'];
        }
        return p;
    }
    TrieNode* root = NULL;
public:
    /** Initialize your data structure here. */
    Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        TrieNode* p = root;
        for(int i = 0; word[i]; i++) {
            if(p->next[word[i]-'a'] == NULL)
                p->next[word[i]-'a'] = new TrieNode();
            p = p->next[word[i]-'a'];
        }
        p->isWord = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        TrieNode* p = find(word);
        if(p == NULL || p->isWord == false)
            return false;
        return true;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        return find(prefix) != NULL;
    }
};
```