---
title: Sword To Offer 041 - 和为S的连续正数序列
date: 2018-04-17 18:14:51
categories: Sword To Offer
---
# 和为S的连续正数序列

<!--more-->

## Desicription

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

## Solution

```cpp
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int>> result;
        vector<int> currentVector;
        int currentSum = 0;
        for(int i = 1; i <= sum; i++) {
            currentSum += i;
            currentVector.push_back(i);
            if(sum == currentSum && currentVector.size() >= 2) {
                result.push_back(currentVector);
            }
            bool change = false;
            while(currentSum > sum) {
                int front = currentVector.front();
                currentVector.erase(currentVector.begin());
                currentSum -= front;
                change = true;
            }
            if(sum == currentSum && currentVector.size() >= 2 && change) {
                result.push_back(currentVector);
            }
        }
        return result;
    }
};
```