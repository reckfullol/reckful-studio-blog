---
title: LeetCode 0241 - Different Ways to Add Parentheses
date: 2018-12-09 00:12:08
categories: LeetCode
---
# Different Ways to Add Parentheses

<!--more-->

## Desicription

Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are `+`, `-` and `*`.

Example 1:

```
Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```

Example 2:

```
Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

## Solution

```cpp
class Solution {
public:
    vector<int> diffWaysToCompute(const string &input) {
        vector<int> res;
        for(int i = 0; input[i]; i++) {
            if(input[i] == '+' || input[i] == '-' || input[i] == '*') {
                vector<int> nums_1 = diffWaysToCompute(input.substr(0, static_cast<unsigned int>(i)));
                vector<int> nums_2 = diffWaysToCompute(input.substr(static_cast<unsigned int>(i + 1)));
                for(const auto& a : nums_1) {
                    for(const auto& b : nums_2) {
                        if(input[i] == '+') {
                            res.push_back(a + b);
                        } else if(input[i] == '-') {
                            res.push_back(a - b);
                        } else {
                            res.push_back(a * b);
                        }
                    }
                }
            }
        }
        if(res.empty()) {
            res.push_back(atoi(input.c_str()));
        }
        return res;
    }
};
```