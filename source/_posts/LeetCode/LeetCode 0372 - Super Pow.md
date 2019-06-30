---
title: LeetCode 0372 - Super Pow
date: 2019-06-30 18:07:13
categories: LeetCode
---
# Super Pow

<!--more-->

## Desicription

Your task is to calculate a^b mod 1337 where a is a positive integer and b is an extremely large positive integer given in the form of an array.

**Example 1:**

```
Input: a = 2, b = [3]
Output: 8
```

**Example 2:**

```
Input: a = 2, b = [1,0]
Output: 1024
```

## Solution

```cpp
class Solution {
public:
    int superPow(int a, std::vector<int>& b) {
        auto pow = [](long long a, long long b) {
            long long res = 1;
            while(b) {
                if(b & 1) {
                    res = (res * a) % 1337;
                }
                b >>= 1;
                a = (a * a) % 1337;
            }
            return res;
        };

        long long res = 1;
        for(int i = 0; i < b.size(); i++) {
            res = pow(res, 10) * pow(a, b[i]) % 1337;
        }
        return res;
    }
};
```