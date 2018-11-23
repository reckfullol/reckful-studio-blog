---
title: LeetCode 0202 - Happy Number
date: 2018-06-12 11:59:12
categories: LeetCode
---
# Happy Number

<!--more-->

## Desicription

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Example**: 

```
Input: 19
Output: true
Explanation: 
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 02 = 1
```

## Solution

```cpp
class Solution {
private:
    int fun(int n) {
        int sum = 0;
        while(n) {
            sum += (n%10) * (n%10);
            n /= 10;
        }
        return sum;
    }
public:
    bool isHappy(int n) {
        int slow = n;
        int fast = n;
        do {
            slow = fun(slow);
            fast = fun(fast);
            fast = fun(fast);
        } while(fast != slow);
        if(slow == 1)
            return true;
        else
            return false;
    }
};
```