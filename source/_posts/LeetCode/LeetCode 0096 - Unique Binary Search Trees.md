---
title: LeetCode 0096 - Unique Binary Search Trees
date: 2017-12-15 14:23:34
categories: LeetCode
---
# Unique Binary Search Trees #

<!--more-->

## Desicription ##

Given *n*, how many structurally unique **BST's** (binary search trees) that store values 1...*n*?

For example,
Given *n* = 3, there are a total of 5 unique **BST's**.

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

## Solution ##

```cpp
class Solution {
public:
    int numTrees(int n) {
        long long res = 1;
        for(int i = n + 1; i <= 2*n; i++)
            res = res*i/(i-n);
        return res/(n+1);
    }
};
```