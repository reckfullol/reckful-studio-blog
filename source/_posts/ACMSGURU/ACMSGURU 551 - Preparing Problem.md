---
title: ACMSGURU 551 - Preparing Problem
date: 2019-12-09 00:47:47
categories: ACMSGURU
---
# Preparing Problem

<!--more-->

## Problem Description


It is not easy to prepare a problem for a programming contest. Petya and Vasya decided that problem "A+B" needs at least n distinct solutions to be written. It doesn't matter how many solutions each of them will write, they need to write at least n solutions in total. We know that Petya needs t1 units of time to write a solution, and Vasya needs t2 units of time. They start to work simultaneously at time 0. Thus, for example, Petya finishes writing his first solution at time t1, his second solution at 2 · t1 and so on.

Petya and Vasya are working by the same algorithm. Each time Petya (Vasya) finishes writing a solution, he checks on how many solutions have already been written up to the current time moment t. Ready solutions are the solutions that have been fully written by this time. The solutions that were fully finished exactly at time t are also considered ready. If the number of such solutions is strictly less than n, then Petya (Vasya) starts writing the next solution. If a member of the jury began working on a problem, he doesn't stop working under any circumstances, and he will surely finish it.

Petya and Vasya realize that if they act on this algorithm, they will not necessarily write exactly n solutions in total. Maybe they'll write more solutions.

Considering that Petya and Vasya work non-stop, find, how many solutions they wrote in total and the moment when the latest solution was finished. The latest solution is one which was finished last.

## Input

The only input line contains three integers n, t1 and t2 (1 ≤ n, t1, t2 ≤ 5000).

## Output

Print two integers — m and f, where m is the number of written solutions, and f is the moment when the last solution was finished.

## Example(s)

|sample input|sample output|
|--|--|
|5 2 3|5 6|

|sample input|sample output|
|-|-|
|5 2 4|6 8|

|sample input|sample output|
|-|-|
|3 30 50|4 100|

## Note
In the first sample Petya finished his solutions at time 2, 4 and 6, and Vasya — at time 3 and 6. They finished writing their last solutions simultaneously, at time 6, and at this exact moment they already had the total of 5 written solutions and stopped working.

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
#define int long long
    std::ios::sync_with_stdio(false);
    int n, t1, t2;
    std::cin >> n >> t1 >> t2;

    if(t1 > t2) {
        std::swap(t1, t2);
    }

    int left = 1;
    int right = n;
    int CountT1 = 0;
    while(left <= right) {
        int mid = (left + right) / 2;
        int currentCount = mid + (mid * t1) / t2;
        if(currentCount < n) {
            left = mid + 1;
        } else {
            right = mid - 1;
            CountT1 = mid;
        }
    }

    int CountT2 = CountT1 * t1 / t2;
    CountT2 += (CountT1 + CountT2 <= n && CountT2 * t2 < CountT1 * t1) ? 1 : 0;

    std::cout << CountT1 + CountT2  << " " << std::max(t1 * CountT1, t2 * CountT2) << std::endl;
    return 0;
}
```