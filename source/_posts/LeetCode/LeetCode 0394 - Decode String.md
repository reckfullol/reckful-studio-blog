---
title: LeetCode 0394 - Decode String
date: 2019-09-07 15:57:29
categories: LeetCode
---
# Decode String

<!--more-->

## Desicription

Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

**Examples:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

## Solution

```cpp
class Solution {
public:
    std::string decodeString(const std::string& s) {
        std::stack<std::pair<int, std::string>> stack = {};
        stack.push({0, ""});
        int count = 0;
        for(const auto& c: s) {
            if(c >= '0' && c <= '9') {
                count = count * 10 + c - '0';
            } else if((c >= 'a' && c <= 'z') || (c >= 'A' && c <= 'Z')) {
                stack.top().second += c;
            } else if(c == '[') {
                stack.push({count, ""});
                count = 0;
            } else if(c == ']') {
                auto top = stack.top();
                stack.pop();
                while(top.first--) {
                    stack.top().second += top.second;
                }
            }
        }
        return stack.top().second;
    }
};
```