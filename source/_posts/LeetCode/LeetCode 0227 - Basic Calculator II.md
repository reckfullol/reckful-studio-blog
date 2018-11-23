---
title: LeetCode 0227 - Basic Calculator II
date: 2018-07-05 17:53:14
categories: LeetCode
---
# Basic Calculator II

<!--more-->

## Desicription

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only **non-negative** integers, `+, -, *, /` operators and empty spaces ` `. The integer division should truncate toward zero.

**Example 1:**

```
Input: "3+2*2"
Output: 7
```

**Example 2:**

```
Input: " 3/2 "
Output: 1
```

**Example 3:**

```
Input: " 3+5 / 2 "
Output: 5
```

**Note:**

- You may assume that the given expression is always valid.
- Do not use the eval built-in library function.

## Solution

```cpp
class Solution {
public:
    long long calculate(string s) {
        stack<long long> numbers;
        long long number = 0;
        char sign = '+';
        for(int i = 0; s[i]; i++) {
            if(s[i] >= '0' && s[i] <= '9') {
                number = number * 10 + s[i] - '0';
            }
            if(!(s[i] >= '0' && s[i] <= '9') && s[i] != ' ' || i == s.size() - 1) {
                if(sign == '+') {
                    numbers.push(number);
                } else if(sign == '-') {
                    numbers.push(-number);
                } else if(sign == '*') {
                    long long top_number = numbers.top();
                    numbers.pop();
                    numbers.push(top_number * number);
                } else if(sign == '/') {
                    long long top_number = numbers.top();
                    numbers.pop();
                    numbers.push(top_number / number);
                }
                sign = s[i];
                number = 0;
            }
        }
        long long result = 0;
        while(!numbers.empty()) {
            result += numbers.top();
            numbers.pop();
        }
        return result;
    }
};
```