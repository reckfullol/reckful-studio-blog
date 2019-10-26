---
title: ACMSGURU 105 - Div 3
date: 2019-10-26 17:35:18
categories: ACMSGURU
---
# Div 3

<!--more-->

## Problem Description

There is sequence 1, 12, 123, 1234, ..., 12345678910, ... . Given first N elements of that sequence. You must determine amount of numbers in it that are divisible by 3.

## Input

Input contains N (1<=N<=2^31 - 1).

## Output

Write answer to the output.

## Sample Input

4

## Sample Output

2

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    int n{};
    std::cin >> n;
    std::cout << n / 3 * 2 + (n % 3 > 1) << std::endl;
    return 0;
}
```