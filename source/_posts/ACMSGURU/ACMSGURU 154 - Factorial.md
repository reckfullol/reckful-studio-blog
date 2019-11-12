---
title: ACMSGURU 154 - Factorial
date: 2019-11-12 19:43:17
categories: ACMSGURU
---
# Factorial

<!--more-->

## Problem Description

You task is to find minimal natural number N, so that N! contains exactly Q zeroes on the trail in decimal notation. As you know N! = 1*2*...*N. For example, 5! = 120, 120 contains one zero on the trail.

## Input

One number Q written in the input (0<=Q<=10^8).

## Output

Write "No solution", if there is no such number N, and N otherwise.

## Sample test(s)

## Input

2

## Output

10

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int q;
    std::cin >> q;

    if(q == 0) {
        std::cout << 1 << std::endl;
    }

    long long left = 5;
    long long right = 5 * q;
    long long res = -1;
    while(left <= right) {
        long long mid = (left + right) / 2;
        long long tmpMid = mid;
        int count = 0;
        while(tmpMid != 0) {
            tmpMid /= 5;
            count += tmpMid;
        }
        if(count == q) {
            res = mid;
            right = mid - 1;
        } else if(count > q) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    if(res == -1) {
        std::cout << "No solution" << std::endl;
    } else {
        std::cout << res << std::endl;
    }

    return 0;
}
```