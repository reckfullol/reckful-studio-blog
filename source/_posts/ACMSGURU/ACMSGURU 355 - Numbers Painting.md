---
title: ACMSGURU 355 - Numbers Painting
date: 2020-12-17 11:33:31
categories: ACMSGURU
---
# Numbers Painting

<!--more-->

## Problem Description

Dr. Vasechkin wants to paint all numbers from 1 to N in such a way that if number A is divisible by number B, numbers A and B have different colors.

Help Dr. Vasechkin to find such a painting, where the number of the colors used is minimal.

### Input

The input contains integer number N (![](https://espresso.codeforces.com/8fec68e0b1e065e37b2bf8fff6f989e3eda3f5b3.png)).

### Output

Write the number of the colors M in the desired painting in the first line of the output. In the second line of the output write the desired painting of numbers from 1 to N. The used colors should be represented by numbers from 1 to M. If there are several solutions, choose any of them.

### Example(s)

|sample input|sample output|
|--|--|
|12|4<br>1 2 2 3 2 3 2 4 3 3 2 4|

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    int n;
    std::cin >> n;
    std::vector<int> nums(n + 1, 1);
    int max_num = 1;
    for(int i = 2; i <= n; i++) {
        int num = i;
        int cnt = 0;
        for(int j = 2; j * j <= num; j++) {
            while(num % j == 0) {
                cnt += 1;
                num /= j;
            }
        }
        nums[i] += cnt;
        nums[i] += num > 1;
        max_num = std::max(max_num, nums[i]);
    }
    std::cout << max_num << std::endl;
    for(int i = 1; i <= n; i++) {
        std::cout << nums[i] << " ";
    }
    return 0;
}
```