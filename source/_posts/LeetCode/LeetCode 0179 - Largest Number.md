---
title: LeetCode 0179 - Largest Number
date: 2018-06-04 20:26:02
categories: LeetCode
---
# Largest Number

<!--more-->

## Desicription

Given a list of non negative integers, arrange them such that they form the largest number.

**Example** 1:

```
Input: [10,2]
Output: "210"
```

**Example** 2:

```
Input: [3,30,34,5,9]
Output: "9534330"
```

**Note**: The result may be very large, so you need to return a string instead of an integer.

## Solution

```cpp
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        vector<string> s;
        for(auto it : nums)
            s.push_back(to_string(it));
        sort(s.begin(), s.end(), [](string a, string b){return a + b > b + a;});
        string res;
        for(auto it : s)
            res += it;
        while(res[0] == '0' && res.size() > 1)
            res.erase(0, 1);
        return res;
    }
};
```