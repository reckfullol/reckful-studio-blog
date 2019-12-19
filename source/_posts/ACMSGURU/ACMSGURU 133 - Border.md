---
title: ACMSGURU 398 - Friends of Friends
date: 2019-12-18 16:59:43
categories: ACMSGURU
---
# Friends of Friends

<!--more-->

## Problem Description

Along the border between states A and B there are N defence outposts. For every outpost k, the interval [Ak,Bk] which is guarded by it is known. Because of financial reasons, the president of country A decided that some of the outposts should be abandoned. In fact, all the redundant outposts will be abandoned. An outpost i is redundant if there exists some outpost j such that Aj<Ai and Bi<Bj. Your task is to find the number of redundant outposts.

## Input

The first line of the input will contain the integer number N (1<=N<=16 000). N lines will follow, each of them containing 2 integers: Ak and Bk (0<= Ak < Bk <= 2 000 000 000), separated by blanks. All the numbers Ak will be different. All the numbers Bk will be different.


## Output

You should print the number of redundant outposts.

## Sample Input

```
5
0 10
2 9
3 8
1 15
6 11
```

## Sample Output

```
3
```

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
#define int long long
    std::ios::sync_with_stdio(false);

    int n;
    std::cin >> n;

    std::vector<std::pair<int, int>> outposts(n, std::pair<int, int>{});

    for(int i = 0; i < n; i++) {
        std::cin >> outposts[i].first >> outposts[i].second;
    }

    std::sort(outposts.begin(), outposts.end(), [](std::pair<int, int> a, std::pair<int, int> b) {
        return a.first < b.first;
    });

    int max_index = 0;
    int count = 0;
    for(int i = 0; i < outposts.size(); i++) {
        if(outposts[i].second > outposts[max_index].second) {
            max_index = i;
        }
        if(outposts[i].first > outposts[max_index].first && outposts[i].second < outposts[max_index].second) {
            count += 1;
        }
    }

    std::cout << count << std::endl;
    return 0;
}
```
