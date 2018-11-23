---
title: Sword To Offer 064 - 滑动窗口的最大值
date: 2018-05-15 11:37:03
categories: Sword To Offer
---
# 滑动窗口的最大值

<!--more-->

## Desicription

给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

## Solution

```cpp
class Solution {
public:
    vector<int> maxInWindows(const vector<int>& num, unsigned int size) {
        if(size > num.size() || num.empty() || size <= 0) {
            return vector<int>();
        }
        deque<int> maxQueue;
        vector<int> res;
        for(int i = 0; i < size; i++) {
            if(!maxQueue.empty() && num[maxQueue.back()] <= num[i]) {
                maxQueue.pop_back();
            }
            maxQueue.push_back(i);
        }
        for(int i = size; i < num.size(); i++) {
            res.push_back(num[maxQueue.front()]);
            while(!maxQueue.empty() && maxQueue.front() <= i - size) {
                maxQueue.pop_front();
            }
            while(!maxQueue.empty() && num[maxQueue.back()] <= num[i]) {
                maxQueue.pop_back();
            }
            maxQueue.push_back(i);
        }
        res.push_back(num[maxQueue.front()]);
        return res;
    }
};
```