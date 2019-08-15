---
title: LeetCode 0386 - Lexicographical Numbers
date: 2019-08-15 21:54:15
categories: LeetCode
---
# Lexicographical Numbers

<!--more-->

## Desicription

Given an integer n, return 1 - n in lexicographical order.

For example, given 13, return: [1,10,11,12,13,2,3,4,5,6,7,8,9].

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

## Solution

```cpp
class Solution {
public:
    std::vector<int> lexicalOrder(int n) {
        auto stringVector = std::vector<std::string>(n);
        for(int i = 1; i <= n; i++) {
            stringVector[i - 1] = std::to_string(i);
        }

        std::sort(stringVector.begin(), stringVector.end(), std::less<std::string>{});
        auto intVector = std::vector<int>(n);
        for(int i = 0; i < intVector.size(); i++) {
            intVector[i] = std::stoi(stringVector[i]);
        }

        return intVector;
    }
};
```