---
title: ACMSGURU 180 - Inversions
date: 2019-10-28 22:37:17
categories: ACMSGURU
---
# Inversions

<!--more-->

## Problem Description

There are N integers (1<=N<=65537) A1, A2,.. AN (0<=Ai<=10^9). You need to find amount of such pairs (i, j) that 1<=i<j<=N and A[i]>A[j].

## Input

The first line of the input contains the number N. The second line contains N numbers A1...AN.

## Output

Write amount of such pairs.

## Sample test(s)

## Input

5
2 3 1 5 4

## Output

3

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int n{};
    std::cin >> n;

    std::vector<int> vec(n, 0);
    std::vector<int> tmp(n, 0);

    for(int i = 0; i < n; i++) {
        std::cin >> vec[i];
    }

    long long res{0};

    std::function<void(int, int)> mergeSort;
    mergeSort = [&](int left, int right){
        if(right - left > 1) {
            int mid = (left + (right - left) / 2);
            int p = left;
            int q = mid;
            int index = left;
            mergeSort(left, mid);
            mergeSort(mid, right);
            while(p < mid || q < right) {
                if(q >= right || (p < mid && vec[p] <= vec[q])) {
                    tmp[index++] = vec[p++];
                } else {
                    tmp[index++] = vec[q++];
                    res += mid - p;
                }
            }
            for(index = left; index < right; index++) {
                vec[index] = tmp[index];
            }
        }
    };

    mergeSort(0, n);

    std::cout << res;

    return 0;
}
```