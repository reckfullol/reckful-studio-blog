---
title: ACMSGURU 231 - Prime Sum
date: 2019-11-11 21:21:46
categories: ACMSGURU
---
# Prime Sum

<!--more-->

## Problem Description

Find all pairs of prime numbers (A, B) such that A<=B and their sum is also a prime number and does not exceed N.

## Input

The input of the problem consists of the only integer N (1<=N<=10^6).

## Output

On the first line of the output file write the number of pairs meeting the requirements. Then output all pairs one per line (two primes separated by a space).

## Sample test(s)

## Input

4

## Output

0

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    int n{};
    std::cin >> n;
    std::vector<bool> visit(n + 1, false);
    std::vector<int> prime{};

    for(unsigned long i = 2; i < visit.size(); i++) {
        if(visit[i]) {
            continue;
        }
        prime.push_back(i);
        for(unsigned long j = i * 2; j < visit.size(); j += i) {
            visit[j] = true;
        }
    }

    int count = 0;
    for(unsigned long i = 1; i < prime.size(); i++) {
        if(prime[i] - prime[i - 1] == 2) {
            count += 1;
        }
    }
    std::cout << count << std::endl;

    for(unsigned long i = 1; i < prime.size(); i++) {
        if(prime[i] - prime[i - 1] == 2) {
            std::cout << 2 << " " << prime[i - 1] << std::endl;
        }
    }

    return 0;
}
```