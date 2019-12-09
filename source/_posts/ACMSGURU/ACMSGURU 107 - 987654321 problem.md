---
title: ACMSGURU 107 - 987654321 problem
date: 2019-12-09 23:18:28
categories: ACMSGURU
---
# 987654321 problem

<!--more-->

## Problem Description

For given number N you must output amount of N-digit numbers, such, that last digits of their square is equal to 987654321.

## Input

Input contains integer number N (1<=N<=106)

## Output

Write answer to the output.

## Sample Input

8

## Sample Output

0

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int n = 0;
    std::cin >> n;
    if(n <= 8) {
        std::cout << 0 << std::endl;
    } else if(n == 9) {
        std::cout << 8 << std::endl;
    } else {
        std::cout << 72;
        while(n---10) {
            std::cout << "0";
        }
        std::cout << std::endl;
    }

    return 0;
}
```
