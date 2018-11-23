---
title: Sword To Offer 028 - 数组中出现次数超过一半的数字
date: 2018-03-27 15:18:38
categories: Sword To Offer
---
# 剑指Offer 028 - 数组中出现次数超过一半的数字

<!--more-->

## Desicription

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

## Solution

```cpp
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        if(numbers.empty()) {
            return 0;
        }
        int temp = numbers[0];
        int count = 1;
        for(int i = 1; i < numbers.size(); i++) {
            numbers[i] == temp ? count++ : count--;
            if(count == 0) {
                temp = numbers[i];
                count = 1;
            }
        }
        count = 0;
        for (int number : numbers) {
            count += temp == number;
        }
        return count > numbers.size() / 2 ? temp : 0;
    }
};
```