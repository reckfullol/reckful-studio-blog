---
title: LeetCode 0332 - Reconstruct Itinerary
date: 2019-06-01 14:30:44
categories: LeetCode
---
# Reconstruct Itinerary

<!--more-->

## Desicription

Given a list of airline tickets represented by pairs of departure and arrival airports [from, to], reconstruct the itinerary in order. All of the tickets belong to a man who departs from JFK. Thus, the itinerary must begin with JFK.

Note:

If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
All airports are represented by three capital letters (IATA code).
You may assume all tickets form at least one valid itinerary.
**Example 1:**

```
Input: [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
Output: ["JFK", "MUC", "LHR", "SFO", "SJC"]
```

**Example 2:**

```
Input: [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"].
             But it is larger in lexical order.
```

## Solution

```cpp
class Solution {
public:
    std::vector<std::string> findItinerary(std::vector<std::vector<std::string>>& tickets) {
        auto graph = std::map<std::string, std::multiset<std::string>>();
        for(const auto& ticket : tickets) {
            graph[ticket[0]].insert(ticket[1]);
        }
        auto res = std::vector<std::string>();
        dfs("JFK", graph, res);

        return std::vector<std::string>(res.rbegin(), res.rend());
    }

private:
    static void dfs(const std::string& current, std::map<std::string, std::multiset<std::string>>& graph, std::vector<std::string>& res) {
        while(!graph[current].empty()) {
            auto next = *graph[current].begin();
            graph[current].erase(graph[current].begin());
            dfs(next, graph, res);
        }

        res.push_back(current);
    }
};
```