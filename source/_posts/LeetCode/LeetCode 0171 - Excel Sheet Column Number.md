---
title: LeetCode 0171 - Excel Sheet Column Number
date: 2018-06-04 12:09:27
categories: LeetCode
---
# Excel Sheet Column Number

<!--more-->

## Desicription

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:

```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

**Example** 1:

```
Input: "A"
Output: 1
```

**Example** 2:

```
Input: "AB"
Output: 28
```

**Example** 3:

```
Input: "ZY"
Output: 701
```

## Solution

```cpp
class Solution {
public:
    int titleToNumber(string s) {
        int res = 0;
        for(int i = s.size() - 1; i >= 0; i--)
            res += pow(26, s.size() - 1 - i) * (s[i] - 'A' + 1);
        return res;
    }
};
```