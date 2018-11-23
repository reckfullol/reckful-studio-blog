---
title: LeetCode 0168 - Excel Sheet Column Title
date: 2018-06-04 12:05:32
categories: LeetCode
---
# Excel Sheet Column Title

<!--more-->

## Desicription

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
```

**Example** 1:

```
Input: 1
Output: "A"
```

**Example** 2:

```
Input: 28
Output: "AB"
```

**Example** 3:

```
Input: 701
Output: "ZY"
```

## Solution

```cpp
class Solution {
public:
    string convertToTitle(int n) {
        string res;
        while(n) {
            res = string(1, (n - 1) % 26 + 'A') + res;
            n = (n - 1) / 26;
        }
        return res;
    }
};
```