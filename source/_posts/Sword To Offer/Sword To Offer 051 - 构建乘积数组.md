---
title: Sword To Offer 051 - 构建乘积数组
date: 2018-05-11 21:32:45
categories: Sword To Offer
---
# 构建乘积数组

<!--more-->

## Desicription

给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

## Solution

```cpp
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        vector<int> prefix(A.size(), 1);
        vector<int> suffix(A.size(), 1);
        vector<int> result(A.size(), 1);
        for(int i = 0; i < A.size(); i++) {
            prefix[i] = i ? A[i] * prefix[i - 1] : prefix[i];
        }
        for(int i = A.size() - 1; i >= 0; i--) {
            suffix[i] = i == A.size() - 1 ? A[i] : suffix[i + 1] * A[i];
        }
        for(int i = 0; i < A.size(); i++) {
            if(i - 1 >= 0) {
                result[i] *= prefix[i - 1];
            }
            if(i + 1 < A.size()) {
                result[i] *= suffix[i + 1];
            }
        }
        return result;
    }
};
```