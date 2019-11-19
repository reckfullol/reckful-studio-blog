---
title: ACMSGURU 113 - Nearly prime numbers
date: 2019-11-19 23:24:15
categories: ACMSGURU
---
# Nearly prime numbers

<!--more-->

## Problem Description

Nearly prime number is an integer positive number for which it is possible to find such primes P1 and P2 that given number is equal to P1*P2. There is given a sequence on N integer positive numbers, you are to write a program that prints “Yes” if given number is nearly prime and “No” otherwise.

## Input

Input file consists of N+1 numbers. First is positive integer N (1£N£10). Next N numbers followed by N. Each number is not greater than 109. All numbers separated by whitespace(s).

## Output

Write a line in output file for each number of given sequence. Write “Yes” in it if given number is nearly prime and “No” in other case.

## Sample Input

1
6

## Sample Output

Yes

## Solution

```cpp
#include <bits/stdc++.h>

bool checkPrime(int num) {
    for(int i = 2; i <= std::sqrt(num) + 1; i++) {
        if(num % i == 0) {
            return false;
        }
    }
    return true;
}

int main() {
    std::ios::sync_with_stdio(false);
    long long maxn = std::sqrt(1e10);
    std::vector<bool> visit = std::vector<bool>(maxn, false);
    std::vector<int> prime{};

    for(unsigned int i = 2; i < visit.size(); i++) {
        if(visit[i] == true) {
            continue;
        }
        prime.push_back(i);
        for(unsigned long j = i * 2; j < visit.size(); j += i) {
            visit[j] = true;
        }
    }

    int n;
    std::cin >> n;
    while(n--) {
        int num;
        std::cin >> num;
        bool flag = false;
        for(const auto& primeNum : prime) {
            if(primeNum >= num) {
                continue;
            }
            if(num % primeNum != 0) {
                continue;
            }
            int quotient = num / primeNum;
            if(quotient > prime.back()) {
                if(checkPrime(quotient) == true) {
                    flag = true;
                    break;
                }
            } else {
                auto res = std::lower_bound(prime.begin(), prime.end(), num / primeNum);
                if(res != prime.end() && *res * primeNum == num) {
                    flag = true;
                    break;
                }
            }
        }
        std::cout << (flag ? "Yes" : "No") << std::endl;
    }
    return 0;
}
```