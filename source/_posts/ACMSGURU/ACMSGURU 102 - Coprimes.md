---
title: ACMSGURU 102 - Coprimes
date: 2019-10-24 15:04:37
categories: ACMSGURU
---
# Coprimes

<!--more-->

## Problem Description

For given integer N (1<=N<=10^4) find amount of positive numbers not greater than N that coprime with N. Let us call two positive integers (say, A and B, for example) coprime if (and only if) their greatest common divisor is 1. (i.e. A and B are coprime iff gcd(A,B) = 1).

## Input

Input file contains integer N.

## Output

Write answer in output file.

## Sample Input

```
9
```

## Sample Output

```
6
```

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int n;
    std::cin >> n;

    int ans = n;
    for(int i = 2; i * i <= n; i++) {
        if(n % i == 0) {
            ans -= ans / i;
            while(n % i == 0) {
                n /= i;
            }
        }
    }

    if(n > 1) {
        ans -= ans / n;
    }

    std::cout << ans << std::endl;

    return 0;
}
```