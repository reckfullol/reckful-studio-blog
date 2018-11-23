---
title: LeetCode 0017 - Letter Combinations of a Phone Number
date: 2017-11-09 20:09:26
categories: LeetCode
---
# Letter Combinations of a Phone Number #

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

![fuck](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

```
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note:**

Although the above answer is in lexicographical order, your answer could be in any order you want.

## Solution ##

```cpp
class Solution {
private:
    vector<string>mp = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};    
public:
    vector<string> letterCombinations(string digits) {
        vector<string> res;
        if(digits.empty())
            return res;
        res.push_back("");
        for(int i = 0; digits[i]; i++){
            vector<string> tmp;
            for(int j = 0; mp[digits[i]-'0'][j]; j++)
                for(int k = 0; k < res.size(); k++)
                    tmp.push_back(res[k] + mp[digits[i] - '0'][j]);
            res = tmp;
        }
        return res;
    }
};
```