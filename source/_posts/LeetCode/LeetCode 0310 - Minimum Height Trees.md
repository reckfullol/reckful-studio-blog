---
title: LeetCode 0310 - Minimum Height Trees
date: 2019-05-13 23:21:26
categories: LeetCode
---
# Minimum Height Trees

<!--more-->

## Desicription

For an undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

**Format**

The graph contains n nodes which are labeled from 0 to n - 1. You will be given the number n and a list of undirected edges (each edge is a pair of labels).

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

**Example 1 :**

```

Input: n = 4, edges = [[1, 0], [1, 2], [1, 3]]

        0
        |
        1
       / \
      2   3 

Output: [1]

```

**Example 2 :**

```

Input: n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

     0  1  2
      \ | /
        3
        |
        4
        |
        5 

Output: [3, 4]

```

**Note:**

- According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”
- The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

## Solution

```cpp
class Solution {
public:
    std::vector<int> findMinHeightTrees(int n, std::vector<std::vector<int>>& edges) {
        auto graph = std::unordered_map<int, std::unordered_set<int>>();
        for(const auto& edge : edges) {
            graph[edge[0]].insert(edge[1]);
            graph[edge[1]].insert(edge[0]);
        }

        auto leaf = std::queue<int>();
        for(const auto& point : graph) {
            if(point.second.size() == 1) {
                leaf.push(point.first);
            }
        }
        while(graph.size() > 2) {

            auto leafCount = leaf.size();
            while(leafCount--) {
                const auto point = leaf.front();
                leaf.pop();

                graph[*graph[point].begin()].erase(point);
                if(graph[*graph[point].begin()].size() == 1) {
                    leaf.push(*graph[point].begin());
                }
                graph.erase(point);
            }
        }

        auto res = std::vector<int>();
        for(const auto& point : graph) {
            res.push_back(point.first);
        }

        return res.empty() ? std::vector<int>{0} : res;
    }
};
```