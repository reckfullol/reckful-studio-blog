---
title: ACMSGURU 123 - The sum
date: 2019-10-25 18:10:15
categories: ACMSGURU
---
# The sum

<!--more-->

## Problem Description

The Fibonacci sequence of numbers is known: F1 = 1; F2 = 1; Fn+1 = Fn + Fn-1, for n>1. You have to find S - the sum of the first K Fibonacci numbers.

## Input

First line contains natural number K (0<K<41).

## Output

First line should contain number S.

## Sample Input

5

## Sample Output

12

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int k;
    std::cin >> k;

    unsigned long long a = 1;
    unsigned long long b = 1;

    unsigned long long res = 0;
    for(int i = 1; i <= k; i++) {
        if(!(i == 1 || i == 2)) {
            auto tmp = a;
            a += b;
            b = tmp;
        }
        res += a;
    }

    std::cout << res << std::endl;

    return 0;
}
```