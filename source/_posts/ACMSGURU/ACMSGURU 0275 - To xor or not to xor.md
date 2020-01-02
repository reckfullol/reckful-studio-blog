---
title: ACMSGURU 0275 - To xor or not to xor
date: 2020-01-02 21:48:28 
categories: ACMSGURU
---
# To xor or not to xor

<!--more-->

## Problem Description

The sequence of non-negative integers A1, A2, ..., AN is given. You are to find some subsequence Ai1, Ai2, ..., Aik (1 <= i1 < i2 < ... < ik <= N) such, that Ai1 XOR Ai2 XOR ... XOR Aik has a maximum value.

## Input
The first line of the input file contains the integer number N (1 <= N <= 100). The second line contains the sequence A1, A2, ..., AN (0 <= Ai <= 10^18).

## Output

Write to the output file a single integer number -- the maximum possible value of Ai1 XOR Ai2 XOR ... XOR Aik.

## Sample test(s)

### Input

3

11 9 5

### Output

14

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int n;
    std::cin >> n;

    auto nums = std::vector<long long>(n);
    for(auto& num : nums) {
        std::cin >> num;
    }

    long long res = 0;

    for(int index = 63; index >= 0; index--) {
        for(int i = 0; i < nums.size(); i++) {
            if(!(nums[i] & (1LL << index))) {
                continue;
            }
            if(!(res & (1LL << index))) {
                res ^= nums[i];
            }

            for(int j = 0; j < nums.size(); j++) {
                if((nums[j] & (1LL << index)) && j != i) {
                    nums[j] ^= nums[i];
                }
            }
            nums[i] = 0;
        }
    }

    std::cout << res << std::endl;
    return 0;
}
```

