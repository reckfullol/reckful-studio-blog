---
title: Sword To Offer 033 - 丑数
date: 2018-03-30 19:36:43
categories: Sword To Offer
---
# 丑数

<!--more-->

## Desicription

把只包含因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

## Solution

```cpp
class Solution {
public:
    long long GetUglyNumber_Solution(int index) {
        if(index == 0) {
            return 0;
        }
        set<long long> s;
        int token = 0;
        s.insert(1);
        while(token + 1 < index) {
            auto currentPoint = s.begin();
            long long currentNumber = *currentPoint;
            s.erase(currentPoint);
            token++;
            s.insert(currentNumber * 2);
            s.insert(currentNumber * 3);
            s.insert(currentNumber * 5);
        }
        return *s.begin();
    }
};
```