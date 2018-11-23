---
title: LeetCode 0049 - Group Anagrams
date: 2017-11-12 21:13:35
categories: LeetCode
---
# Group Anagrams #

<!--more-->

## Desicription ##

Given an array of strings, group anagrams together.

For example, given: `["eat", "tea", "tan", "ate", "nat", "bat"]`, 
Return:

```
[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**Note:** All inputs will be in lower-case.

## Solution ##

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        sort(strs.begin(), strs.end());
        map<string, vector<string>> mp;
        int len = strs.size();
        for(int i = 0; i < len; i++){
            string tmp = strs[i];
            sort(tmp.begin(), tmp.end());
            mp[tmp].push_back(strs[i]);
        }
        vector<vector<string>> res;
        for(auto it = mp.begin(); it != mp.end(); it++)
            res.push_back(it->second);
        return res;
    }
};
```