---
title: Sword To Offer 010 - 矩形覆盖
date: 2018-03-15 17:30:58
categories: Sword To Offer
---
# 矩形覆盖

<!--more-->

## Desicription

我们可以用2\*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2\*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

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
    long long rectCover(int n) {
        if(n == 0) {
            return 0;
        }
        mat a = {
            {1, 1},
            {1, 0}
        };
        a = Pow(a, n + 1);
        return a[1][0];
    }
};
```
