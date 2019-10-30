---
title: ACMSGURU 184 - Patties
date: 2019-10-29 20:44:05
categories: ACMSGURU
---
# Patties

<!--more-->

## Problem Description

TsPetya is well-known with his famous cabbage patties. Petya's birthday will come very soon, and he wants to invite as many guests as possible. But the boy wants everybody to try his specialty of the house. That's why he needs to know the number of the patties he can cook using the stocked ingredients. Petya has P grams of flour, M milliliters of milk and C grams of cabbage. He has plenty of other ingredients. Petya knows that he needs K grams of flour, R milliliters of milk and V grams of cabbage to cook one patty. Please, help Petya calculate the maximum number of patties he can cook.

## Input
The input file contains integer numbers P, M, C, K, R and V, separated by spaces and/or line breaks (1 <= P, M, C, K, R, V <= 10000).

## Output
Output the maximum number of patties Petya can cook.

## Sample test(s)

### Input

```
3000 1000 500
30 15 60
```

### Output

```
8
```

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int a{}, b{}, c{};
    int x{}, y{}, z{};

    std::cin >> a >> b >> c >> x >> y >> z;

    std::cout << std::min(std::min(a / x, b / y), c / z);

    return 0;
}
```