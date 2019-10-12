---
title: LeetCode 0405 - Convert a Number to Hexadecimal
date: 2019-10-12 20:29:48
categories: LeetCode
---
# Convert a Number to Hexadecimal

<!--more-->

## Desicription

Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

**Note:**

1. All letters in hexadecimal (a-f) must be in lowercase.
2. The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
3. The given number is guaranteed to fit within the range of a 32-bit signed integer.
4. You must not use any method provided by the library which converts/formats the number to hex directly.

**Example 1:**

```
Input:
26

Output:
"1a"
```

**Example 2:**

```
Input:
-1

Output:
"ffffffff"
```

## Solution

```cpp
class Solution {
public:
    std::string toHex(unsigned int num) {
        if(num == 0) {
            return std::string{"0"};
        }
        std::string res{};
        while(num != 0) {
            auto tmp = num & 0xf;
            if(tmp >= 10) {
                res += 'a' + tmp - 10;
            } else {
                res += '0' + tmp;
            }
            num >>= 4;
        }
        std::reverse(res.begin(), res.end());
        return res;
    }
};
```