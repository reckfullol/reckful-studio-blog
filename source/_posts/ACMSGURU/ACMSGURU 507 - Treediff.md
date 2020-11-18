---
title: ACMSGURU 507 - Treediff
date: 2020-11-18 10:43:31
categories: ACMSGURU
---
# Treediff

<!--more-->

## Problem Description

Andrew has just made a breakthrough in complexity theory: he thinks that he can prove P=NP if he can get a data structure which allows to perform the following operation quickly. Naturally, you should help him complete his brilliant research. Consider a rooted tree with integers written in the leaves. For each internal (non-leaf) node v of the tree you must compute the minimum absolute difference between all pairs of numbers written in the leaves of the subtree rooted at v.

### Input

The first line of the input file contains two integers n and m — overall number of nodes in the tree and number of leaves in the tree respectively. ![](https://espresso.codeforces.com/e969819d22422c7ba2a9a9664d60c3a425d6cdc5.png). All nodes are numbered from 1 to n. Node number 1 is always the root of the tree. Each of the other nodes has a unique parent in the tree. Each of the next n - 1 lines of the input file contains one integer — the number of the parent node for nodes 2, 3,..., n respectively. Each of the last m lines of the input file contains one integer ranging from ![](https://espresso.codeforces.com/8956ddde38feeac9a82239162072525d790b1870.png) to ![](https://espresso.codeforces.com/4d18431e5cbe3d02088d40704fae7e3d35398fb3.png) — the value of the corresponding leaf. Leaves of the tree have numbers from n - m + 1 to n.

### Output

Output one line with n - m integers: for each internal node of the tree output the minimum absolute difference between pairs of values written in the leaves of its subtree. If there is only one leaf in the subtree of some internal node, output number 231 - 1 for that node. Output the answers for the nodes in order from node number 1 to n - m.

### Example(s)

|sample input|sample output|
|--|--|
|5 4<br>1<br>1<br>1<br>1<br>1<br>4<br>7<br>9<br>|2|

|sample input|sample output|
|--|--|
|5 4<br>1<br>1<br>1<br>1<br>1<br>4<br>7<br>10|3|

|sample input|sample output|
|--|--|
|7 4<br>1<br>2<br>1<br>2<br>3<br>3<br>2 <br>10<br>7<br>15|3 3 8|

|sample input|sample output|
|--|--|
|2 1<br>1<br>100|2147483647|

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

#define int int64_t

    int n, m;
    std::cin >> n >> m;

    std::vector<std::vector<int>> sons_index(n + 1, std::vector<int>{});
    std::vector<std::set<int>> sons_value(n + 1, std::set<int, std::less<int>>{});
    std::vector<int> values(n + 1, INT_MAX);

    for(int i = 2; i <= n; i++) {
        int current_father;
        std::cin >> current_father;
        sons_index[current_father].push_back(i);
    }

    for(int i = n - m + 1; i <= n; i++) {
        int value;
        std::cin >> value;
        sons_value[i].insert(value);
    }

    auto dfs = std::function<void(int)>{};
    dfs = [&](int index) -> void {
        for(const auto& son_index : sons_index[index]) {
            dfs(son_index);

            values[index] = std::min(values[index], values[son_index]);
            if(sons_value[son_index].size() > sons_value[index].size()) {
                std::swap(sons_value[son_index], sons_value[index]);
            }
            for(auto it = sons_value[son_index].begin(); it != sons_value[son_index].end(); it++) {
                auto pre = sons_value[index].lower_bound(*it);
                auto suc = pre;
                if(pre != sons_value[index].begin()) {
                    pre--;
                }
                if(pre != sons_value[index].end()) {
                    values[index] = std::min(values[index], std::abs(*it - *pre));
                }
                if(suc != sons_value[index].end()) {
                    values[index] = std::min(values[index], std::abs(*it - *suc));
                }
                sons_value[index].insert(*it);
            }
        }
    };
    dfs(1);

    for(int i = 1; i <= n; i++) {
        if(!sons_index[i].empty()) {
            std::cout << values[i] << " ";
        }
    }
}
```