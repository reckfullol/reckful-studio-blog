---
title: LeetCode 0374 - Find K Pairs with Smallest Sums
date: 2019-06-30 22:00:23
categories: LeetCode
---
# Find K Pairs with Smallest Sums

<!--more-->

## Desicription

We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API guess(int num) which returns 3 possible results (-1, 1, or 0):

```
-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!
```

**Example :**

```
Input: n = 10, pick = 6
Output: 6
```

## Solution

```cpp
// Forward declaration of guess API.
// @param num, your guess
// @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
int guess(int num);

class Solution {
public:
    int guessNumber(int n) {
        long long left = 1;
        long long right = n;
        while(true) {
            long long mid = (left + right) >> 1;
            if(guess(mid) == 0) {
                return mid;
            } else if(guess(mid) == 1) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
};
```