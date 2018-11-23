---
title: LeetCode 0204 - Count Primes
date: 2018-06-12 12:04:27
categories: LeetCode
---
# Count Primes

<!--more-->

## Desicription

Count the number of prime numbers less than a non-negative number, **n**.

**Example**:

```
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

## Solution

```cpp
class Solution {
public:
    int countPrimes(int n) {
        vector<bool> notPrime(n, false);
        int cnt = 0;
        for(int i = 2; i < n; i++) {
            if(notPrime[i] == false) {
                cnt++;
                for(int j = 2; i*j < n; j++)
                    notPrime[i*j] = true;
            }
        }
        return cnt;
    }
};
```