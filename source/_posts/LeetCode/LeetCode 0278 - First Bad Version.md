---
title: LeetCode 0278 - First Bad Version
date: 2018-12-16 15:00:32
categories: LeetCode
---
# First Bad Version

<!--more-->

## Desicription

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example:**

```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 
```

## Solution

```cpp
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    long long firstBadVersion(long long n) {
        long long left = 1;
        long long right = n;
        long long res = n;
        while(left <= right) {
            long long mid = (left + right) >> 1;
            if(isBadVersion(mid)) {
                res = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return res;
    }
};
```