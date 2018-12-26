---
title: LeetCode 0306 - Additive Number
date: 2018-12-26 11:03:50
categories: LeetCode
---
# Additive Number

<!--more-->

## Desicription

Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain at least three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

Given a string containing only digits '0'-'9', write a function to determine if it's an additive number.

Note: Numbers in the additive sequence cannot have leading zeros, so sequence 1, 2, 03 or 1, 02, 3 is invalid.

**Example 1:**

```
Input: "112358"
Output: true 
Explanation: The digits can form an additive sequence: 1, 1, 2, 3, 5, 8. 
             1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```

**Example 2:**

```
Input: "199100199"
Output: true 
Explanation: The additive sequence is: 1, 99, 100, 199. 
             1 + 99 = 100, 99 + 100 = 199
```

**Follow up:**

How would you handle overflow for very large input integers?

## Solution

```cpp
class Solution {
public:
    bool isAdditiveNumber(string num) {
        for(int i = 1; i <= num.size() / 2; i++) {
            for(int j = 1; j <= (num.size() - i) / 2; j++) {
                if(check(num.substr(0, i), num.substr(i, j), num.substr(i + j))) {
                    return true;
                }
            }
        }
        return false;
    }

    bool check(string num1, string num2, string num) {
        if((num1.size() > 1  && num1[0] == '0') || (num2.size() > 1 && num2[0] == '0')) {
            return false;
        }
        string sum = add(num1, num2);
        if(num == sum) {
            return true;
        }
        if(num.size() <= sum.size() || sum != num.substr(0, sum.size())) {
            return false;
        }
        return check(num2, sum, num.substr(sum.size()));
    }

    string add(string a, string b) {
        int i = a.size() - 1;
        int j = b.size() - 1;
        int carry = 0;
        string res;
        while(i >= 0 || j >= 0) {
            int sum = carry + (i >= 0 ? a[i--] - '0' : 0) + (j >= 0 ? b[j--] - '0' : 0);
            res += sum % 10 + '0';
            carry = sum / 10;
        }
        if(carry != 0) {
            res += carry + '0';
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```