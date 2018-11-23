---
title: LeetCode 0093 - Restore IP Addresses
date: 2017-12-15 11:43:44
categories: LeetCode
---
# Restore IP Addresses #

<!--more-->

## Desicription ##

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:
Given `"25525511135"`,

return `["255.255.11.135", "255.255.111.35"]`. (Order does not matter)

## Solution ##

```cpp
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> res;
        for(int a = 1; a <= 3; a++) {
            for(int b = 1; b <= 3; b++){
                for(int c = 1; c <= 3; c++) {
                    for(int d = 1; d <= 3; d++){
                        if(a + b + c + d == s.size()){
                            int A = stoi(s.substr(0, a));
                            int B = stoi(s.substr(a, b));
                            int C = stoi(s.substr(a+b, c));
                            int D = stoi(s.substr(a+b+c, d));
                            if(A <= 255 && B <= 255 && C <= 255 && D <= 255){
                                string cur = to_string(A) + "." + to_string(B) + "." + to_string(C) + "." + to_string(D);
                                if(cur.size() == s.size() + 3)
                                    res.push_back(cur); 
                            }
                        }
                    }
                }
            }
        }
        return res;
    }
};
```