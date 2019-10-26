---
title: ACMSGURU 403 - Scientific Problem
date: 2019-10-26 15:20:40
categories: ACMSGURU
---
# Scientific Problem

<!--more-->

## Problem Description

Once upon a time Professor Idioticideasinventor was travelling by train. Watching cheerless landscape outside the window, he decided to invent the theme of his new scientific work. All of a sudden a brilliant idea struck him: to develop an effective algorithm finding an integer number, which is x times less than the sum of all its integer positive predecessors, where number x is given. As far as he has no computer in the train, you have to solve this difficult problem.

## Input

The first line of the input file contains an integer number x (1 ? x ? 10^9).

## Output

Output an integer number — the answer to the problem.

## Example(s)

|sample input|sample output|
|---|---|
|1|3|
|2|5|

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int x{};
    std::cin >> x;

    std::cout << (x << 1 | 1) << std::endl;
    return 0;
}
```

