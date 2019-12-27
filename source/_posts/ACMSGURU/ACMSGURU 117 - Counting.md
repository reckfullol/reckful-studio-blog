---
title: ACMSGURU 117 - Counting
date: 2019-12-19 23:30:48
categories: ACMSGURU
---
# Counting

<!--more-->

## Problem Description

Find amount of numbers for given sequence of integer numbers such that after raising them to the M-th power they will be divided by K.

## Input

Input consists of two lines. There are three integer numbers N, M, K (0<N, M, K<10001) on the first line. There are N positive integer numbers − given sequence (each number is not more than 10001) − on the second line.

## Output

Write answer for given task.

## Sample Input

```
4 2 50
9 10 11 12
```

## Sample Output

```
1
```

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    auto factors = [](int x) -> std::map<int, int> {
        std::map<int, int> res{};
        for(int i = 2; i * i <= x; i++) {
            if(x % i == 0) {
                int cnt = 0;
                while(x % i == 0) {
                    x /= i;
                    cnt += 1;
                }
                res[i] = cnt;
            }
        }
        if(x != 1) {
            res[x] = 1;
        }
        return res;
    };

    int n, m, k;
    std::cin >> n >> m >> k;
    auto k_factors = factors(k);

    int count = 0;
    while(n--) {
        int num;
        std::cin >> num;
        auto num_factors = factors(num);
        bool is_divided = true;
        for(const auto& k_factor : k_factors) {
            auto it = num_factors.find(k_factor.first);
            if(it == num_factors.end() || it->second * m < k_factor.second) {
                is_divided = false;
                break;
            }
        }
        count += is_divided;
    }

    std::cout << count << std::endl;
    return 0;
}
```
