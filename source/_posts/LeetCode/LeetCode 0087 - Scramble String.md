---
title: LeetCode 0087 - Scramble String
date: 2017-11-21 09:34:41
categories: LeetCode
---
# Scramble String #

<!--more-->

## Desicription ##


Given a string `s1`, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of `s1` = `"great"`:

```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```

To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node `"gr"` and swap its two children, it produces a scrambled string `"rgeat"`.

```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```

We say that `"rgeat"` is a scrambled string of `"great"`.

Similarly, if we continue to swap the children of nodes `"eat"` and `"at"`, it produces a scrambled string `"rgtae"`.

```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```

We say that `"rgtae"` is a scrambled string of `"great"`.

Given two strings `s1` and `s2` of the same length, determine if `s2` is a scrambled string of `s1`.

## Solution ##

```cpp
class Solution {
public:
    bool isScramble(string s1, string s2) {
        if(s1 == s2)
            return 1;
        vector<int> cnt(26, 0);
        for(int i = 0; s1[i]; i++)
            cnt[s1[i] - 'a']++, cnt[s2[i] - 'a']--;
        for(int i = 0; i < 26; i++)
            if(cnt[i])
                return 0;
        for(int i = 1; s1[i]; i++){
            if(isScramble(s1.substr(0, i), s2.substr(0, i)) && isScramble(s1.substr(i), s2.substr(i)))
                return 1;
            if(isScramble(s1.substr(0, i), s2.substr(s1.size() - i)) && isScramble(s1.substr(i), s2.substr(0, s1.size() - i)))
                return 1;
        }
        return 0;
    }
};
```