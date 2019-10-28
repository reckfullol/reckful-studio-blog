---
title: ACMSGURU 358 - Median of Medians
date: 2019-10-28 21:17:04
categories: ACMSGURU
---
# Median of Medians

<!--more-->

## Problem Description

Vasya learned definition of median of three numbers. He says, "Median of three numbers is the number located in the middle when numbers are ordered in non-descending order". Subtle Pete gave him much more difficult task. Vasya has to find median of each of three triples and then find the median of three numbers he found. Please help Vasya with the task.

## Input

The input file contains three lines. Each line contains three integers. Each number is not less than -1000 and is not greater than 1000.

## Output

Print one number - median of three medians.

## Example(s)

|sample input|sample output|
|--|--|
|6 4 5<br>7 9 8<br>1 2 3<br>|5|

|sample input|sample output|
|--|--|
|1 2 2<br>4 3 2<br>2 3 4<br>|3|

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    std::vector<int> vec(3, 0);
    std::vector<int> final(3, 0);
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            std::cin >> vec[j];
        }
        std::sort(vec.begin(), vec.end());
        final[i] = vec[1];
    }

    std::sort(final.begin(), final.end());

    std::cout << final[1] << std::endl;

    return 0;
}
```