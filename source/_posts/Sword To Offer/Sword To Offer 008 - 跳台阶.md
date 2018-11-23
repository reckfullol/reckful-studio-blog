---
title: Sword To Offer 008 - 跳台阶
date: 2018-03-15 15:03:41
categories: Sword To Offer
---
# 跳台阶

<!--more-->

## Desicription

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

## Solution

```cpp
class Solution {
private:
    typedef vector<vector<long long>> mat;

    mat Mul(mat& a, mat& b) {
        mat c(a.size(), vector<long long>(b[0].size(), 0));
        for(int i = 0; i < a.size(); i++) {
            for(int k = 0; k < a[0].size(); k++) {
                for(int j = 0; j < b[0].size(); j++) {
                    c[i][j] += a[i][k] * b[k][j];
                }
            }
        }
        return c;
    }

    mat Pow(mat a, int n) {
        mat b(a.size(), vector<long long>(a[0].size(), 0));
        for(int i = 0; i < b.size(); i++) {
            b[i][i] = 1;
        }
        while(n) {
            if(n & 1) {
                b = Mul(b, a);
            }
            a = Mul(a, a);
            n >>= 1;
        }
        return b;

    }
public:
    long long jumpFloor(int n) {
        mat a = {
            {1, 1},
            {1, 0}
        };
        a = Pow(a, n + 1);
        return a[1][0];
    }
};
```