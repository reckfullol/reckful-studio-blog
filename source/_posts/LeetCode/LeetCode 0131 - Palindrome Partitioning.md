---
title: LeetCode 0131 - Palindrome Partitioning
date: 2018-05-25 20:26:28
categories: LeetCode
---
# Palindrome Partitioning

<!--more-->

## Desicription

Given a string *s*, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of *s*.

**Example**:

```
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```

## Solution

```cpp
class Solution {
private:
    void dfs(int index, string& s, vector<string>& path, vector<vector<string>>& res) {
        if(index == s.size()) {
            res.push_back(path);
            return ;
        }
        for(int i = index; s[i]; i++) {
            if(isPalindrome(s, index, i)) {
                path.push_back(s.substr(index, i - index + 1));
                dfs(i + 1, s, path, res);
                path.pop_back();
            }
        }
    }
    bool isPalindrome(string& s, int left, int right) {
        while(left <= right)
            if(s[left++] != s[right--])
                return false;
        return true;
    }
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> res;
        vector<string> path;
        dfs(0, s, path, res);
        return res;
    }
};
```