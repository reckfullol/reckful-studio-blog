---
title: LeetCode 0119 - Pascal's Triangle II
date: 2017-12-18 13:20:44
categories: LeetCode
---
# Pascal's Triangle II #

<!--more-->

## Desicription ##

Given an index *k*, return the k*th* row of the Pascal's triangle.

For example, given *k* = 3,
Return `[1,3,3,1]`.

**Note:**
Could you optimize your algorithm to use only O(*k*) extra space?

## Solution ##

```cpp
class Solution {
private:
    int C(double A, double B) {
        double res = 1;
        for(int i = 1; i <= B; i++)
            res *= (double)(A-i+1)/i;
        return res+0.5;
    }
public:
    vector<int> getRow(int rowIndex) {
        vector<int> res;
        for(int i = 0; i <= rowIndex; i++)
            res.push_back(C(rowIndex, i));
        return res;
    }
};
```