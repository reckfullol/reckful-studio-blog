---
title: LeetCode 0258 - Add Digits
date: 2018-12-11 09:25:00
categories: LeetCode
---
# Add Digits

<!--more-->

## Desicription

Given a non-negative integer `num`, repeatedly add all its digits until the result has only one digit.

**Example:**

```
Input: 38
Output: 2 
Explanation: The process is like: 3 + 8 = 11, 1 + 1 = 2. 
             Since 2 has only one digit, return it.
```

## Solution

```cpp
class Solution {
private:
    int getLength(int num) {
        return static_cast<int>(log10(num) + 1);
    }
public:
    int addDigits(int num) {
        if(getLength(num) <= 1) {
            return num;
        }
        int sum = 0;
        while(num) {
            sum += num % 10;
            num /= 10;
        }
        return addDigits(sum);
    }
};
```