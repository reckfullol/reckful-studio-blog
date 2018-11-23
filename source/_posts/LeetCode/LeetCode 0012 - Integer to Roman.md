---
title: LeetCode 0012 - Integer to Roman
date: 2017-11-09 19:45:46
categories: LeetCode
---
# Integer to Roman #

<!--more-->

## Desicription ##

Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

## Solution ##

```cpp
class Solution {
public:
    string intToRoman(int num) {
        string thousand[4] = {"", "M", "MM", "MMM"};
        string hundred[10] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
        string ten[10] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
        string one[10] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
        return thousand[num/1000] + hundred[(num%1000)/100] + ten[(num%100)/10] + one[num%10];
    }
};
```