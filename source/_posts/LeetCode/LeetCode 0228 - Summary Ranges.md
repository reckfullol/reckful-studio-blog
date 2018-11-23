---
title: LeetCode 0228 - Summary Ranges
date: 2018-07-05 18:35:00
categories: LeetCode
---
# Summary Ranges

<!--more-->

## Desicription

Given a sorted integer array without duplicates, return the summary of its ranges.

**Example 1:**

```
Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.
```

**Example 2:**

```
Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
```

## Solution

```cpp
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> result;
        int number1 = 1;
        int number2 = 0;
        for(auto& number : nums) {
            if(number1 == number2 + 1) {
                number1 = number;
                number2 = number;
            } else {
                if(number == number2 + 1) {
                    number2++;
                } else {
                    if(number1 == number2) {
                        result.push_back(to_string(number1));
                    } else {
                        result.push_back(to_string(number1) + "->" + to_string(number2));
                    }
                    number1 = number;
                    number2 = number;
                }
            }
        }
        if(number1 == number2) {
            result.push_back(to_string(number1));
        } else if(number1 != number2 + 1) {
            result.push_back(to_string(number1) + "->" + to_string(number2));
        }
        return result;
    }
};
```