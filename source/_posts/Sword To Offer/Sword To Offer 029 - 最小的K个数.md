---
title: Sword To Offer 029 - 最小的K个数
date: 2018-03-27 15:29:58
categories: Sword To Offer
---
# 最小的K个数

<!--more-->

## Desicription

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

## Solution

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.size() < k) {
            return vector<int>(0);
        }
        auto compare = [](int a, int b) {
            return b > a;
        };
        priority_queue<int, vector<int>, decltype(compare)> priorityQueue(compare);
        for(auto number : input) {
            priorityQueue.push(number);
            while(priorityQueue.size() > k) {
                priorityQueue.pop();
            }
        }
        vector<int> res;
        while(!priorityQueue.empty()) {
            res.push_back(priorityQueue.top());
            priorityQueue.pop();
        }
        return res;
    }
};
```