---
title: LeetCode 0395 - Longest Substring with At Least K Repeating Characters
date: 2019-09-19 18:12:54
categories: LeetCode
---
# Longest Substring with At Least K Repeating Characters

<!--more-->

## Desicription

Find the length of the longest substring T of a given string (consists of lowercase letters only) such that every character in T appears no less than k times.

**Example 1:**

```
Input:
s = "aaabb", k = 3

Output:
3

The longest substring is "aaa", as 'a' is repeated 3 times.
```

**Example 2:**

```
Input:
s = "ababbc", k = 2

Output:
5

The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

## Solution

```cpp
class Solution {
public:
    int longestSubstring(const std::string& s, const int& k) {
        return longestSubstring(s, k, 0, static_cast<int>(s.size()) - 1);
    }

    int longestSubstring(const std::string& s, const int& k, int left, int right) {
        if(left > right) {
            return 0;
        }

        auto count = std::vector<int>(26, 0);
        for(int i = left; i <= right; i++) {
            count[s[i] - 'a']++;
        }

        int res = right - left + 1;
        for(int i = left; i <= right; i++) {
            if(count[s[i] - 'a'] > 0 && count[s[i] - 'a'] < k) {
                res = std::max(longestSubstring(s, k, left, i - 1), longestSubstring(s, k, i + 1, right));
                return res;
            }
        }
        return res;
    }
};
```