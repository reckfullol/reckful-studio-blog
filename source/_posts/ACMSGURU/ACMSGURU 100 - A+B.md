---
title: ACMSGURU 100 - A+B
date: 2019-10-21 17:44:45
categories: ACMSGURU
---
# A+B

<!--more-->

## Problem Description

Read integers A and B from input file and write their sum in output file.

## Input

Input file contains A and B (0<A,B<10001).

## Output

Write answer in output file.

## Sample Input

```
5 3
```

## Sample Output

```
8
```

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    unsigned short a{};
    unsigned short b{};

    std::cin >> a >> b;
    std::cout << a + b << std::endl;
}
```