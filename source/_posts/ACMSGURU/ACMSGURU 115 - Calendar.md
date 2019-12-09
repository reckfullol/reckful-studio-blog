---
title: ACMSGURU 115 - Calendar
date: 2019-12-10 00:01:15
categories: ACMSGURU
---
# Calendar

<!--more-->

## Problem Description

First year of new millenium is gone away. In commemoration of it write a program that finds the name of the day of the week for any date in 2001.

## Input

Input is a line with two positive integer numbers N and M, where N is a day number in month M. N and M is not more than 100.

## Output

Write current number of the day of the week for given date (Monday – number 1, … , Sunday – number 7) or phrase “Impossible” if such date does not exist.

## Sample Input

21 10

## Sample Output

7

## Solution

```cpp
#include <bits/stdc++.h>

int main() {

    std::ios::sync_with_stdio(false);

    std::vector<int> month{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    int n, m;
    std::cin >> n >> m;
    if(m > 12 || n > month[m - 1]) {
        std::cout << "Impossible";
    }

    int day = 0;
    for(int i = 0; i < m - 1; i++) {
        day += month[i];
    }
    day += n - 1;

    std::cout << day % 7 + 1 << std::endl;
}
```
