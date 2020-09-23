---
title: ACMSGURU 130 - Circle
date: 2020-09-23 14:19:21
categories: ACMSGURU
---
# Circle

<!--more-->

## Problem Description

On a circle border there are 2k different points A1, A2, ..., A2k, located contiguously. These points connect k chords so that each of points A1, A2, ..., A2k is the end point of one chord. Chords divide the circle into parts. You have to find N - the number of different ways to connect the points so that the circle is broken into minimal possible amount of parts P.

## Input

The first line contains the integer k (1 <= k <= 30).

## Output

The first line should contain two numbers N and P delimited by space.

## Sample Input

2

## Sample Output

2 3

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
//    std::vector<unsigned long long> dp(35, 0);
//    dp[1] = 1;
//    dp[2] = 2 * dp[1] * dp[1];
//    for(int i = 3; i <= 30; i++) {
//        dp[i] += 2 * dp[1] * dp[i - 1];
//        for(int j = 1; j <= i - 1; j++) {
//            dp[i] += dp[1] * dp[j] * dp[i - 1 - j];
//        }
//    }
//    for(int i = 1; i <= 30; i++) {
//        std::cout << i << " " << dp[i] << std::endl;
//    }
    std::vector<std::string> dp {
        "0",
        "1",
        "2",
        "5",
        "14",
        "42",
        "132",
        "429",
        "1430",
        "4862",
        "16796",
        "58786",
        "208012",
        "742900",
        "2674440",
        "9694845",
        "35357670",
        "129644790",
        "477638700",
        "1767263190",
        "6564120420",
        "24466267020",
        "91482563640",
        "343059613650",
        "1289904147324",
        "4861946401452",
        "18367353072152",
        "69533550916004",
        "263747951750360",
        "1002242216651368",
        "3814986502092304",
    };
    int k;
    std::cin >> k;
    std::cout << dp[k] << " " << k + 1 << std::endl;
    return 0;
}
```