---
title: ACMSGURU 134 - Centroid
date: 2020-01-24 19:41:39
categories: ACMSGURU
---
# Centroid

<!--more-->

## Problem Description

You are given an undirected connected graph, with N vertices and N-1 edges (a tree). You must find the centroid(s) of the tree.
In order to define the centroid, some integer value will be assosciated to every vertex. Let's consider the vertex k. If we remove the vertex k from the tree (along with its adjacent edges), the remaining graph will have only N-1 vertices and may be composed of more than one connected components. Each of these components is (obviously) a tree. The value associated to vertex k is the largest number of vertices contained by some connected component in the remaining graph, after the removal of vertex k. All the vertices for which the associated value is minimum are considered centroids.

## Input

The first line of the input contains the integer number N (1<=N<=16 000). The next N-1 lines will contain two integers, a and b, separated by blanks, meaning that there exists an edge between vertex a and vertex b.

## Output

You should print two lines. The first line should contain the minimum value associated to the centroid(s) and the number of centroids. The second line should contain the list of vertices which are centroids, sorted in ascending order.

## Sample Input

```
7
1 2
2 3
2 4
1 5
5 6
6 7
```

## Sample Output

```
3 1
1
```

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
//    std::ios::sync_with_stdio(false);

    int n;
    std::cin >> n;

    std::vector<bool> is_root(n + 1, true);
    std::map<int, std::vector<int>> tree{};
    for(int i = 1; i < n; i++) {
        int a, b;
        std::cin >> a >> b;
        is_root[b] = false;
        tree[a].push_back(b);
    }
    for(int i = 1; i <= n; i++) {
        if(tree.find(i) == tree.end()) {
            tree[i] = std::vector<int>{};
        }
    }

    int root_index = -1;
    for(int i = 1; i <= n; i++) {
        if(is_root[i]) {
            root_index = i;
            break;
        }
    }

    std::vector<int> node_count(n + 1, 0);
    auto dfs_count = std::function<int(int)>{};
    dfs_count = [&dfs_count, &node_count, &tree](int index) -> int {
        if(tree.find(index) == tree.end()) {
            return 0;
        }
        int sum = 0;
        for(int next_index : tree[index]) {
            node_count[next_index] = dfs_count(next_index);
            sum += node_count[next_index];
        }
        return node_count[index] = sum + 1;
    };
    dfs_count(root_index);

    std::vector<int> node_centroid(n + 1, 1);
    int centroid_value = INT_MAX;
    auto dfs_get_centroid = std::function<void(int, int)>{};
    dfs_get_centroid = [&dfs_get_centroid, &node_count, &node_centroid, &centroid_value, &tree, &root_index](int index, int father_index) -> void {
        if(father_index != -1) {
            node_centroid[index] = std::max(node_centroid[index], node_count[root_index] - node_count[index]);
        }
        for(auto child_index : tree[index]) {
            node_centroid[index] = std::max(node_centroid[index], node_count[child_index]);
            dfs_get_centroid(child_index, index);
        }
        centroid_value = std::min(centroid_value, node_centroid[index]);
    };
    dfs_get_centroid(root_index, -1);

    std::vector<int> res{};
    for(int i = 1; i <= n; i++) {
        if(node_centroid[i] == centroid_value) {
            res.push_back(i);
        }
    }
    std::sort(res.begin(), res.end(), std::less<>{});

    std::cout << centroid_value << " " << res.size() << std::endl;
    for(auto index : res) {
        std::cout << index << " ";
    }

    return 0;
}
```
