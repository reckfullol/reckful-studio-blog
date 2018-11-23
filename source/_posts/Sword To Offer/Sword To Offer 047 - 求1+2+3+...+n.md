---
title: Sword To Offer 047 - 求1+2+3+...+n
date: 2018-04-18 22:30:15
categories: Sword To Offer
---
# 求1+2+3+...+n 

<!--more-->

## Desicription

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

## Solution

```cpp
class Solution {
public:
    int Sum_Solution(int n) {
        int ans = n;
        ans && (ans += Sum_Solution(n - 1));
        return ans;
    }
};
```