---
title: LeetCode 0140 - Word Break II
date: 2018-05-29 09:20:41
categories: LeetCode
---
# Word Break II

<!--more-->

## Desicription

Given a **non-empty** string *s* and a dictionary *wordDict* containing a list of **non-empty** words, add spaces in *s* to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

**Note**:

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

**Example** 1:

```
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]
```

**Example** 2:

```
Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.
```

**Example** 3:

```
Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

## Solution

```cpp
class Solution {
private:
    unordered_map<string, vector<string>> mp; 
    unordered_set<string> dict;
    vector<string> combine(string s, vector<string> vec) {
        for(int i = 0; i < vec.size(); i++)
            vec[i] += " " + s;
        return vec;
    }
    vector<string> dfs(string s) {
        if(mp.count(s))
            return mp[s];
        vector<string> res;
        if(dict.count(s))
            res.push_back(s);
        for(int i = 1; i < s.size(); i++) {
            string word = s.substr(i);
            if(dict.count(word)) {
                string rem = s.substr(0, i);
                vector<string> prev = combine(word, dfs(rem));
                res.insert(res.end(), prev.begin(), prev.end());
            }
        }
        mp[s] = res;
        return res;
    }
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        dict = unordered_set<string>(wordDict.begin(), wordDict.end());
        return dfs(s);
    }
};
```