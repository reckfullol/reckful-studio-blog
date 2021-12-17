---
title: ACMSGURU 149 - Computer Network
date: 2021-12-17 16:16:45
categories: ACMSGURU
---
# Computer Network

<!--more-->

## Problem Description

A school bought the first computer some time ago. During the recent years the school bought N-1 new computers. Each new computer was connected to one of settled earlier. Managers of school are anxious about slow functioning of the net and want to know for each computer number Si - maximum distance, for which i-th computer needs to send signal (i.e. length of cable to the most distant computer). You need to provide this information.

## Input

There is natural number N (N<=10000) in the first line of input, followed by (N-1) lines with descriptions of computers. i-th line contains two natural numbers - number of computer, to which i-th computer is connected and length of cable used for connection. Total length of cable does not exceed 10^9. Numbers in lines of input are separated by a space.

## Output

Write N lines in output file. i-th line must contain number Si for i-th computer (1<=i<=N).

## Example(s)

Input
3
1 1
1 2


## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    uint64_t n;
    std::cin >> n;

    const uint64_t NETWORK_NODE_MAX = 1e5 + 7;
    // index, path
    std::vector<std::vector<std::pair<uint64_t , uint64_t >>> network(NETWORK_NODE_MAX, std::vector<std::pair<uint64_t , uint64_t >>{});
    std::vector<std::priority_queue<uint64_t , std::vector<uint64_t >, std::greater<uint64_t >> > result(NETWORK_NODE_MAX, std::priority_queue<uint64_t , std::vector<uint64_t >, std::greater<uint64_t >>{});
    std::vector<std::priority_queue<uint64_t , std::vector<uint64_t >, std::greater<uint64_t >>> dp(NETWORK_NODE_MAX, std::priority_queue<uint64_t , std::vector<uint64_t >, std::greater<uint64_t >>{});

    // max, second max
    auto get_heap_result = [](std::priority_queue<uint64_t , std::vector<uint64_t >, std::greater<uint64_t >>& heap) -> std::pair<uint64_t , uint64_t > {
        if(heap.empty()) {
            return {0, 0};
        }
        uint64_t second_max = heap.top();
        heap.pop();
        if(heap.empty()) {
            heap.push(second_max);
            return {second_max, 0};
        }
        uint64_t max = heap.top();
        heap.push(second_max);
        return {max, second_max};
    };

    for(uint64_t i = 2; i <= n; i++) {
        uint64_t father = 0;
        uint64_t path = 0;
        std::cin >> father >> path;
        network[father].push_back({i, path});
    }

    auto dfs = std::function<uint64_t (uint64_t )>{};
    dfs = [&](uint64_t index) -> uint64_t {
        for(const auto& next : network[index]) {
            dp[index].push(dfs(next.first) + next.second);
            if(dp[index].size() > 2) {
                dp[index].pop();
            }
        }
        return get_heap_result(dp[index]).first;
    };
    dfs(1);

    // index
    std::queue<uint64_t > bfs;
    bfs.push(1);
    result[1] = dp[1];
    while(!bfs.empty()) {
        uint64_t index = bfs.front();
        bfs.pop();
        for(const auto& next : network[index]) {
            uint64_t next_index = next.first;
            uint64_t path = next.second;
            uint64_t father_max_path = get_heap_result(result[index]).first;
            uint64_t father_second_path = get_heap_result(result[index]).second;
            uint64_t son_max_path = get_heap_result(dp[next.first]).first;
            uint64_t son_second_path = get_heap_result(dp[next.first]).second;
            result[next_index].push(son_max_path);
            result[next_index].push(son_second_path);
            result[next_index].push(father_max_path - son_max_path == path ?
                                        path + father_second_path
                                        :
                                        path + father_max_path);
            result[next_index].pop();
            bfs.push(next_index);
        }
    }
    for(uint64_t i = 1; i <= n; i++) {
        std::cout << get_heap_result(result[i]).first << std::endl;
    }
}
```