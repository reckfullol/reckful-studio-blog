---
title: ACMSGURU 222 - Little Rooks
date: 2019-12-16 10:24:27
categories: ACMSGURU
---
# Little Rooks

<!--more-->

## Problem Description

Inspired by a "Little Bishops" problem, Petya now wants to solve problem for rooks.

A rook is a piece used in the game of chess which is played on a board of square grids. A rook can only move horizontally and vertically from its current position and two rooks attack each other if one is on the path of the other.

Given two numbers n and k, your job is to determine the number of ways one can put k rooks on an n × n chessboard so that no two of them are in attacking positions.

## Input

The input file contains two integers n (1 ≤ n ≤ 10) and k (0 ≤ k ≤ n2).

## Output

Print a line containing the total number of ways one can put the given number of rooks on a chessboard of the given size so that no two of them are in attacking positions.

## Sample test(s)

## Input

4 4

## Output

24

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
#define int long long
    std::ios::sync_with_stdio(false);

    int n, k;
    std::cin >> n >> k;

    auto C = [](int n, int k) {
        int res_n = 1;
        int res_k = 1;
        int res_tmp = 1;
        for(int i = 1; i <= n; i++) {
            res_n *= i;
        }
        for(int i = 1; i <= k; i++) {
            res_k *= i;
        }
        for(int i = 1; i <= n - k; i++) {
            res_tmp *= i;
        }
        return static_cast<int>(res_n / (res_k * res_tmp));
    };

    auto A = [](int n, int k) {
        int res_n = 1;
        int res_k = 1;
        for(int i = 1; i <= n; i++) {
            res_n *= i;
        }
        for(int i = 1; i <= n - k; i++) {
            res_k *= i;
        }
        return static_cast<int>(res_n / res_k);
    };

    if(k > n) {
        std::cout << 0 << std::endl;
    } else {
        std::cout << C(n, k) * A(n, k) << std::endl;
    }

    return 0;
}
```
