---
title: Sword To Offer 031 - 整数中1出现的次数（从1到n整数中1出现的次数）
date: 2018-03-30 15:38:00
categories: Sword To Offer
---
# 整数中1出现的次数（从1到n整数中1出现的次数）

<!--more-->

## Desicription

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数。

## Solution

```cpp
class Solution {
private:
    vector<int> digit;
    vector<vector<int>> dp;

    int DigitDP(int length, bool isMax, int count) {
        if(length == 0) {
            return count;
        }
        if(~dp[length][count] && !isMax) {
            return dp[length][count];
        }
        int currentDigit = isMax ? digit[length] : 9;
        int total = 0;
        for(int i = 0; i <= currentDigit; i++) {
            if(i == 1) {
                total += DigitDP(length - 1, isMax && currentDigit == i, count + 1);
            } else {
                total += DigitDP(length - 1, isMax && currentDigit == i, count);
            }
        }
        return isMax ? total : dp[length][count] = total;
    }

public:
    int NumberOf1Between1AndN_Solution(int n) {
        digit = vector<int>(30);
        dp = vector<vector<int>>(30, vector<int>(30, -1));
        int index = 0;
        while(n) {
            digit[++index] = n % 10;
            n /= 10;
        }
        return DigitDP(index, true, 0);
    }
};
```