---
title: LeetCode 0151 - Reverse Words in a String
date: 2018-06-02 11:38:30
categories: LeetCode
---
# Reverse Words in a String

<!--more-->

## Desicription

Given an input string, reverse the string word by word.

**Example**:  

```
Input: "the sky is blue",
Output: "blue is sky the".
```

**Note**:

- A word is defined as a sequence of non-space characters.
- Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.
- You need to reduce multiple spaces between two words to a single space in the reversed string.

**Follow up**: For C programmers, try to solve it in-place in O(1) space.

## Solution

```cpp
class Solution {
public:
    void reverseWords(string &s) {
        string res;
        int right = s.size();
        for(int left = right - 1; left >= 0; left--) {
            if(s[left] == ' ')
                right = left;
            else if(left == 0 || s[left-1] == ' ') {
                if(res.size())
                    res += " ";
                res += s.substr(left, right-left);
            }
        }
        s = res;
    }
};
```