---
title: LeetCode 0295 - Find Median from Data Stream
date: 2018-12-21 15:52:31
categories: LeetCode
---
# Find Median from Data Stream

<!--more-->

## Desicription

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,

`[2,3,4]`, the median is `3`

`[2,3]`, the median is `(2 + 3) / 2 = 2.5`

Design a data structure that supports the following two operations:

- void addNum(int num) - Add a integer number from the data stream to the data structure.
- double findMedian() - Return the median of all elements so far.
 

**Example:**

```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

## Solution

```cpp
class MedianFinder {
private:
    priority_queue<int, vector<int>, greater<>> min_heap;
    priority_queue<int, vector<int>, less<>> max_heap;
public:
    /** initialize your data structure here. */
    MedianFinder() = default;

    void addNum(int num) {
        if(min_heap.size() == max_heap.size()) {
            min_heap.push(num);
            max_heap.push(min_heap.top());
            min_heap.pop();
        } else {
            max_heap.push(num);
            min_heap.push(max_heap.top());
            max_heap.pop();
        }
    }

    double findMedian() {
        return min_heap.size() == max_heap.size() ? (min_heap.top() + max_heap.top()) / 2.0 : max_heap.top();
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */

```