---
title: LeetCode 0282 - Expression Add Operators
date: 2018-12-17 16:03:33
categories: LeetCode
---
# Expression Add Operators

<!--more-->

## Desicription

Given a string that contains only digits 0-9 and a target value, return all possibilities to add **binary** operators (not unary) +, -, or * between the digits so they evaluate to the target value.

**Example 1:**

```
Input: num = "123", target = 6
Output: ["1+2+3", "1*2*3"] 
```

**Example 2:**

```
Input: num = "232", target = 8
Output: ["2*3+2", "2+3*2"]
```

**Example 3:**

```
Input: num = "105", target = 5
Output: ["1*0+5","10-5"]
```

**Example 4:**

```
Input: num = "00", target = 0
Output: ["0+0", "0-0", "0*0"]
```

**Example 5:**

```
Input: num = "3456237490", target = 9191
Output: []
```

## Solution

```cpp
class Solution {
private:
    vector<string> res{};
    void dfs(int index, long cur_sum, string cur_res, char pre_op, long pre_num, const string& num, const long& target) {
        if(index == num.size()) {
            if(cur_sum == target) {
                res.emplace_back(cur_res);
            }
            return ;
        }
        for(int i = index + 1; i <= num.size(); i++) {
            string sub_str = num.substr(index, i-index);
            if(to_string(stol(sub_str)).size() != sub_str.size()) {
                continue;
            }
            dfs(i, cur_sum + stol(sub_str), cur_res + "+" + sub_str, '+', stol(sub_str), num, target);
            dfs(i, cur_sum - stol(sub_str), cur_res + "-" + sub_str, '-', stol(sub_str), num, target);
            dfs(i, pre_op == '-' ? cur_sum + pre_num - pre_num * stol(sub_str) : pre_op == '+' ? cur_sum - pre_num + pre_num * stol(sub_str) : pre_num * stol(sub_str), cur_res + "*" + sub_str, pre_op, pre_num * stol(sub_str), num, target);
        }
    }
public:
    vector<string> addOperators(const string& num, long target) {
        if(!num.empty()) {
            for(int i = 1; i <= num.size(); i++) {
                string sub_str = num.substr(0, static_cast<unsigned int>(i));
                if(to_string(stol(sub_str)).size() != sub_str.size()) {
                    continue;
                }
                dfs(i, stol(sub_str), sub_str, '#', stol(sub_str), num, target);
            }
        }
        return res;
    }
};
```