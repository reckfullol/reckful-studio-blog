---
title: LeetCode 0273 - Integer to English Words
date: 2018-12-15 01:01:46
categories: LeetCode
---
# Integer to English Words

<!--more-->

Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than 2^31 - 1.

**Example 1:**

```
Input: 123
Output: "One Hundred Twenty Three"
```

**Example 2:**

```
Input: 12345
Output: "Twelve Thousand Three Hundred Forty Five"
```

**Example 3:**

```
Input: 1234567
Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
```

**Example 4:**

```
Input: 1234567891
Output: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"
```

## Solution

```cpp
class Solution {
private:
    const vector<string> one_to_nineteen = {
            "",
            "One",
            "Two",
            "Three",
            "Four",
            "Five",
            "Six",
            "Seven",
            "Eight",
            "Nine",
            "Ten",
            "Eleven",
            "Twelve",
            "Thirteen",
            "Fourteen",
            "Fifteen",
            "Sixteen",
            "Seventeen",
            "Eighteen",
            "Nineteen"
    };

    const vector<string> twenty_to_ninety = {
            "",
            "",
            "Twenty",
            "Thirty",
            "Forty",
            "Fifty",
            "Sixty",
            "Seventy",
            "Eighty",
            "Ninety"
    };

    string get_words(int num) {
        if(num >= (int)1e9) {
            return get_words(num / (int)1e9) + " Billion" + get_words(num % (int)1e9);
        } else if(num >= (int)1e6) {
            return get_words(num / (int)1e6) + " Million" + get_words(num % (int)1e6);
        } else if(num >= (int)1e3) {
            return get_words(num / (int)1e3) + " Thousand" + get_words(num % (int)1e3);
        } else if(num >= (int)1e2) {
            return get_words(num / (int)1e2) + " Hundred" + get_words(num % (int)1e2);
        } else if(num >= 20) {
            return string(" ") + twenty_to_ninety[num / 10] + get_words(num % 10);
        } else if(num >= 1) {
            return string(" ") + one_to_nineteen[num];
        } else {
            return string("");
        }
    }

public:
    string numberToWords(int num) {
        if(num == 0) {
            return "Zero";
        } else {
            return get_words(num).substr(1);
        }
    }
};
```
