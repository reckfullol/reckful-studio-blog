---
title: ACMSGURU 101 - Domino
date: 2019-10-24 14:31:56
categories: ACMSGURU
---
# Domino

<!--more-->

## Problem Description

Dominoes – game played with small, rectangular blocks of wood or other material, each identified by a number of dots, or pips, on its face. The blocks usually are called bones, dominoes, or pieces and sometimes men, stones, or even cards.
The face of each piece is divided, by a line or ridge, into two squares, each of which is marked as would be a pair of dice...

The principle in nearly all modern dominoes games is to match one end of a piece to another that is identically or reciprocally numbered.

ENCYCLOPÆDIA BRITANNICA

Given a set of domino pieces where each side is marked with two digits from 0 to 6. Your task is to arrange pieces in a line such way, that they touch through equal marked sides. It is possible to rotate pieces changing left and right side.


## Input

The first line of the input contains a single integer N (1 ≤ N ≤ 100) representing the total number of pieces in the domino set. The following N lines describe pieces. Each piece is represented on a separate line in a form of two digits from 0 to 6 separated by a space.


## Output

Write “No solution” if it is impossible to arrange them described way. If it is possible, write any of way. Pieces must be written in left-to-right order. Every of N lines must contains number of current domino piece and sign “+” or “-“ (first means that you not rotate that piece, and second if you rotate it).

## Sample Input

```
5
1 2
2 4
2 4
6 4
2 1
```

## Sample Output

```
2 -
5 +
1 +
3 +
4 -
```

## Solution

```cpp
#include <bits/stdc++.h>

struct node {
    int right;
    int index;
    inline bool operator< (const node& outNode) {
        return index < outNode.index;
    }
};

std::unordered_map<int, bool> visit{};
std::unordered_map<int, std::vector<node>> nodes{};
std::unordered_map<int, int> degree{};
std::vector<node> path{};


void dfs(const int& left) {
    for(const auto& nextNode : nodes[left]) {
        if(visit[nextNode.index] == true) {
            continue;
        }

        visit[nextNode.index] = true;
        visit[-nextNode.index] = true;
        dfs(nextNode.right);
        path.push_back(nextNode);
    }
}

int main() {
    std::ios::sync_with_stdio(false);

    int n;
    std::cin >> n;

    for(int i = 1; i <= n; i++) {
        int a{};
        int b{};

        std::cin >> a >> b;

        visit[i] = false;
        visit[-i] = false;

        nodes[a].push_back({b, i});
        nodes[b].push_back({a, -i});

        degree[a]++;
        degree[b]++;
    }

    int oddCount = 0;
    int startIndex = degree.begin()->first;
    for(auto const& item : degree) {
        if(item.second & 1) {
            oddCount++;
            startIndex = item.first;
        }
    }

    if(oddCount != 0 && oddCount != 2) {
        std::cout << "No solution" << std::endl;
        return 0;
    }

    dfs(startIndex);

    std::reverse(path.begin(), path.end());

    if(path.size() == n) {
        for(const auto& res : path) {
            std::cout << abs(res.index) << (res.index > 0 ? " +" : " -") << std::endl;
        }
    } else {
        std::cout << "No solution" << std::endl;
    }

    return 0;
}
```