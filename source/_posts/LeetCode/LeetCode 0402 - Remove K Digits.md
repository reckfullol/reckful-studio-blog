---
title: LeetCode 0402 - Remove K Digits
date: 2019-10-11 20:28:31
categories: LeetCode
---
# Remove K Digits

<!--more-->

## Desicription

Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.

**Note:**

- The length of num is less than 10002 and will be ≥ k.
- The given num does not contain any leading zero.

**Example 1:**

```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```

**Example 2:**

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```

**Example 3:**

```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```

## Solution

```cpp
class Solution {
public:
    std::string removeKdigits(const std::string& num, int k) {
        std::stack<char> mono{};
        for(const auto& c : num) {
            while(!mono.empty() && mono.top() > c && k > 0) {
                mono.pop();
                k--;
            }
            mono.push(c);
        }
        while(!mono.empty() && k > 0) {
            mono.pop();
            k--;
        }

        std::string res = std::string(mono.size(), 'a');
        for(int i = mono.size() - 1; i >= 0; i--) {
            res[i] = mono.top();
            mono.pop();
        }
        int index = 0;
        for(; index < res.size(); index++) {
            if(res[index] != '0') {
                break;
            }
        }
        return res.empty() ? std::string{"0"} : res.substr(index).empty() ? std::string{"0"} : res.substr(index);
    }
};
```