---
title: LeetCode 0214 - Shortest Palindrome
date: 2018-06-29 20:12:27
categories: LeetCode
---
# Shortest Palindrome

<!--more-->

## Desicription

Given a string **s**, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

**Example 1:**

```
Input: "aacecaaa"
Output: "aaacecaaa"
```

**Example 2:**

```
Input: "abcd"
Output: "dcbabcd"
```

## Solution

```cpp
class Solution {
public:
    string shortestPalindrome(string s) {
        if(s.size() < 2) {
            return s;
        }

        int max_length = 1;
        for(int i = 0; i <= s.size() / 2; ) {
            int left = i;
            int right = i;
            while(s[i] == s[right + 1]) {
                right++;
            }
            i = right;
            while(left - 1 >= 0 && right + 1 <= s.size() - 1 && s[left - 1] == s[right + 1]) {
                left--;
                right++;
            }
            if(left == 0) {
                max_length = max(max_length, right - left + 1);
            }
            i++;
        }

        string tail_string = s.substr(static_cast<unsigned int>(max_length));
        reverse(tail_string.begin(), tail_string.end());
        return tail_string + s;
    }
};
```