---
title: LeetCode 0139 - Word Break
date: 2018-05-29 08:57:54
categories: LeetCode
---
# Word Break

<!--more-->

## Desicription

Given a **non-empty** string s and a dictionary *wordDict* containing a list of **non-empty** words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

**Note**:

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

**Example** 1:

```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example** 2:

```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

**Example** 3:

```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

## Solution

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        vector<bool> dp(s.size()+1, false);
        dp[0] = true;
        for(int i = 1; i <= s.size(); i++) {
            for(int j = i-1; j >= 0; j--) {
                if(dp[j] && dict.find(s.substr(j, i-j)) != dict.end()) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.size()];
    }
};
```