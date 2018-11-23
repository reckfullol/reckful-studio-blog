---
title: LeetCode 0191 - Number of 1 Bits
date: 2018-06-11 12:15:35
categories: LeetCode
---
# Number of 1 Bits

<!--more-->

## Desicription

Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the [Hamming weight](https://en.wikipedia.org/wiki/Hamming_weight)).

**Example** 1:

```
Input: 11
Output: 3
Explanation: Integer 11 has binary representation 00000000000000000000000000001011 
```

**Example** 2:

```
Input: 128
Output: 1
Explanation: Integer 128 has binary representation 00000000000000000000000010000000
```

## Solution

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int cnt = 0;
        while(n)
            cnt += n & 1, n >>= 1;
        return cnt;
    }
};
```