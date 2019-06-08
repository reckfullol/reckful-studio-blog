---
title: LeetCode 0352 - Data Stream as Disjoint Intervals
date: 2019-06-08 20:41:17
categories: LeetCode
---
# Data Stream as Disjoint Intervals

<!--more-->

## Desicription

Given a data stream input of non-negative integers a1, a2, ..., an, ..., summarize the numbers seen so far as a list of disjoint intervals.

For example, suppose the integers from the data stream are 1, 3, 7, 2, 6, ..., then the summary will be:

```
[1, 1]
[1, 1], [3, 3]
[1, 1], [3, 3], [7, 7]
[1, 3], [7, 7]
[1, 3], [6, 7]
```

Follow up:

What if there are lots of merges and the number of disjoint intervals are small compared to the data stream's size?

NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

## Solution

```cpp
class SummaryRanges {
private:
    std::function<bool(std::pair<int, int>, std::pair<int, int>)> cmp = [](std::pair<int, int> a, std::pair<int, int> b) {
        if(a.first == b.first) {
            return a.second < b.second;
        }
        return a.first < b.first;
    };
    std::set<std::pair<int, int>, decltype(cmp)> set = std::set<std::pair<int, int>, decltype(cmp)>(cmp);

public:
    /** Initialize your data structure here. */
    SummaryRanges() = default;

    void addNum(int val) {
        auto it = set.lower_bound({val, val});
        int left = val;
        int right = val;
        if(it != set.begin() && (--it)->second + 1 < val) {
            it++;
        }

        while(it != set.end() && val + 1 >= it->first && val - 1 <= it->second) {
            left = std::min(left, it->first);
            right = std::max(right, it->second);
            it = set.erase(it);
        }
        set.insert(it, {left, right});
    }

    std::vector<std::vector<int>> getIntervals() {
        auto res = std::vector<std::vector<int>>();
        for(auto num : set) {
            res.push_back({num.first, num.second});
        }
        return res;
    }
};

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges* obj = new SummaryRanges();
 * obj->addNum(val);
 * vector<vector<int>> param_2 = obj->getIntervals();
 */
```