---
title: Sword To Offer 007 - 斐波那契数列
date: 2018-03-15 14:52:58
categories: Sword To Offer
---
# 斐波那契数列

<!--more-->

## Desicription

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。
n<=39

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
    long long Fibonacci(int n) {
        mat a = {
            {1, 1},
            {1, 0}
        };
        a = Pow(a, n);
        return a[1][0];
    }
};
```