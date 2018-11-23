---
title: LeetCode 0224 - Basic Calculator
date: 2018-07-05 16:17:57
categories: LeetCode
---
# Basic Calculator

<!--more-->

## Desicription

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open `(` and closing parentheses `)`, the plus `+` or minus sign `-`, **non-negative** integers and empty spaces ` `.

**Example 1:**

```
Input: "1 + 1"
Output: 2
```

**Example 2:**

```
Input: " 2-1 + 2 "
Output: 3
```

**Example 3:**

```
Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

**Note:**

- You may assume that the given expression is always valid.
- Do not use the eval built-in library function.

## Solution

```cpp
class Solution {
public:
    int calculate(string s) {
        stack<int> sum_stack;
        int result = 0;
        int sign = 1;
        for(int i = 0; s[i]; i++) {
            if(s[i] >= '0' && s[i] <= '9') {
                int number = s[i] - '0';
                while(s[i+1] && s[i+1] >= '0' && s[i+1] <= '9') {
                    i++;
                    number = number * 10 + s[i] - '0';
                }
                result += number * sign;
            } else if(s[i] == '+') {
                sign = 1;
            } else if(s[i] == '-') {
                sign = -1;
            } else if(s[i] == '(') {
                sum_stack.push(result);
                sum_stack.push(sign);
                result = 0;
                sign = 1;
            } else if(s[i] == ')') {
                auto current_sign = sum_stack.top();
                sum_stack.pop();
                auto current_result = sum_stack.top();
                sum_stack.pop();
                result = current_sign * result + current_result;
            }
        }
        return result;
    }
};
```