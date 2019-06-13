---
title: LeetCode 0363 - Max Sum of Rectangle No Larger Than K
date: 2019-06-13 19:48:07
categories: LeetCode
---
# Max Sum of Rectangle No Larger Than K

<!--more-->

## Desicription

Given a non-empty 2D matrix matrix and an integer k, find the max sum of a rectangle in the matrix such that its sum is no larger than k.

**Example:**

```
Input: matrix = [[1,0,1],[0,-2,3]], k = 2
Output: 2 
Explanation: Because the sum of rectangle [[0, 1], [-2, 3]] is 2,
             and 2 is the max number no larger than k (k = 2).
```

**Note:**

1. The rectangle inside the matrix must have an area > 0.
2. What if the number of rows is much larger than the number of columns?

```cpp
class Solution {
public:
    int maxSumSubmatrix(std::vector<std::vector<int>>& matrix, int k) {
        int row = matrix.size();
        if(row == 0) {
            return 0;
        }
        int col = matrix[0].size();
        int res = INT_MIN;

        for(int i = 0; i < col; i++) {
            auto sum = std::vector<int>(row, 0);
            for(int j = i; j < col; j++) {
                for(int z = 0; z < row; z++) {
                    sum[z] += matrix[z][j];
                }

                auto set = std::set<int>();
                set.insert(0);
                int currentSum = 0;
                for(int num : sum) {
                    currentSum += num;
                    auto it = set.lower_bound(currentSum - k);
                    if(it != set.end()) {
                        res = std::max(res, currentSum - *it);
                    }
                    set.insert(currentSum);
                }
            }
        }
        return res;
    }
};
```
