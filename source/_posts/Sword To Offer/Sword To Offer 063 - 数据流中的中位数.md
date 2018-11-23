---
title: Sword To Offer 063 - 数据流中的中位数
date: 2018-05-14 22:58:48
categories: Sword To Offer
---
# 数据流中的中位数

<!--more-->

## Desicription

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

## Solution

```cpp

class Solution {
private:
    int count = 0;
    priority_queue<int, vector<int>, greater<>> minHeap;
    priority_queue<int, vector<int>, less<>> maxHeap;
public:
    void Insert(int num) {
        if(count & 1) {
            minHeap.push(num);
            auto temp = minHeap.top();
            minHeap.pop();
            maxHeap.push(temp);
        } else {
            maxHeap.push(num);
            auto temp = maxHeap.top();
            maxHeap.pop();
            minHeap.push(temp);
        }
        count++;
    }

    double GetMedian() {
        if(count & 1) {
            return minHeap.top();
        } else {
            return (minHeap.top() + maxHeap.top()) / 2.0;
        }
    }
};
```