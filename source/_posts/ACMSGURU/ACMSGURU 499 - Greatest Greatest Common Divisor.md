---
title: ACMSGURU 499 - Greatest Greatest Common Divisor
date: 2019-11-01 17:51:33
categories: ACMSGURU
---
# Greatest Greatest Common Divisor

<!--more-->

## Problem Description

Andrew has just made a breakthrough in sociology: he realized how to predict whether two persons will be good friends or not. It turns out that each person has an inner friendship number (a positive integer). And the quality of friendship between two persons is equal to the greatest common divisor of their friendship number. That means there are prime people (with a prime friendship number) who just can't find a good friend, andWait, this is irrelevant to this problem. You are given a list of friendship numbers for several people. Find the highest possible quality of friendship among all pairs of given people.
Input
The first line of the input file contains an integer n (2 <= n <= 10000) — the number of people to process. The next n lines contain one integer each, between 1 and  1000000(inclusive), the friendship numbers of the given people. All given friendship numbers are distinct.
Output
Output one integer — the highest possible quality of friendship. In other words, output the greatest greatest common divisor among all pairs of given friendship numbers.

## Example(s)

|sample input|sample output|
|--|--|
|4<br>9<br>15<br>25<br>16|5|

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int n{};
    std::cin >> n;

    std::vector<bool> prime(1e6 + 5, false);

    while(n--) {
        int num{};
        std::cin >> num;
        prime[num] = true;
    }


    for(unsigned int i = prime.size() / 2; i >= 1; i--) {
        int cnt = 0;
        for(unsigned int j = i; j < prime.size(); j += i) {
            if(prime[j] == true) {
                cnt += 1;
                if(cnt == 2) {
                    std::cout << i << std::endl;
                    return 0;
                }
            }
        }
    }
    return 0;
}
```


