---
title: LeetCode 0133 - Clone Graph
date: 2018-05-26 13:38:07
categories: LeetCode
---
# LeetCode 0133 - Clone Graph

<!--more-->

## Desicription

Clone an undirected graph. Each node in the graph contains a `label` and a list of its `neighbors`.


**OJ's undirected graph serialization:**

Nodes are labeled uniquely.

We use `#` as a separator for each node, and `,` as a separator for node label and each neighbor of the node.

As an example, consider the serialized graph `{0,1,2#1,2#2,2}`.

The graph has a total of three nodes, and therefore contains three parts as separated by `#`.

1. First node is labeled as `0`. Connect node `0` to both nodes `1` and `2`.
1. Second node is labeled as `1`. Connect node `1` to node `2`.
1. Third node is labeled as `2`. Connect node `2` to node `2` (itself), thus forming a self-cycle.
Visually, the graph looks like the following:

```
       1
      / \
     /   \
    0 --- 2
         / \
         \_/
```

## Solution

```cpp
/**
 * Definition for undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */
class Solution {
private:
    unordered_map<UndirectedGraphNode*, UndirectedGraphNode*> mp;
public:
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if(node == NULL)
            return NULL;
        if(mp.find(node) == mp.end()) {
            mp[node] = new UndirectedGraphNode(node->label);
            for(auto it : node->neighbors) {
                mp[node]->neighbors.push_back(cloneGraph(it));
            }
        }
        return mp[node];
    }
};
```