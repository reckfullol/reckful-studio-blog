---
title: LeetCode 0301 - Remove Invalid Parentheses
date: 2018-12-25 09:55:22
categories: LeetCode
---
# Remove Invalid Parentheses

<!--more-->

## Desicription

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

**Note:** The input string may contain letters other than the parentheses ( and ).

**Example 1:**

```
Input: "()())()"
Output: ["()()()", "(())()"]
```

**Example 2:**

```
Input: "(a)())()"
Output: ["(a)()()", "(a())()"]
```

**Example 3:**

```
Input: ")("
Output: [""]
```

## Solution

```cpp
class Solution {
private:
    bool isInvalid(const string& s) const {
        int count = 0;
        for(const auto& c : s) {
            if(c == '(') {
                count++;
            } else if(c == ')') {
                if(count == 0) {
                    return false;
                }
                count--;
            }
        }
        return count == 0;
    }
public:
    vector<string> removeInvalidParentheses(string s) {
        vector<string> res;
        unordered_set<string> book;
        queue<string> q;
        q.push(s);
        while(!q.empty()) {
            string current_string = q.front();
            q.pop();
            if(book.find(current_string) != book.end()) {
                continue;
            }
            book.insert(current_string);
            if(isInvalid(current_string)) {
                res.push_back(current_string);
            } else if(res.empty()) {
                for(int i = 0; i < current_string.size(); i++) {
                    if(current_string[i] == '(' || current_string[i] == ')') {
                        q.push(current_string.substr(0, static_cast<unsigned int>(i)) + current_string.substr(static_cast<unsigned int>(i + 1)));
                    }
                }
            }
        }
        return res;
    }
};
```