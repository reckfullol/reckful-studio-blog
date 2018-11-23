---
title: LeetCode 0216 - Combination Sum III
date: 2018-06-29 22:18:39
categories: LeetCode
---
# Combination Sum III

<!--more-->

## Desicription

Find all possible combinations of **k** numbers that add up to a number **n**, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Note:**

- All numbers will be positive integers.
- The solution set must not contain duplicate combinations.
**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

## Solution

```cpp
class Solution {
private:
    vector<vector<int>> res;
    void dfs(vector<int>& current_vector, int current_sum, int current_number, const int k, const int n) {
        for(int i = current_number + 1; i <= 9; i++) {
            if(current_vector.size() == k - 1 && current_sum + i == n) {
                current_vector.push_back(i);
                res.push_back(current_vector);
                current_vector.pop_back();
                return ;
            }
            else if(current_vector.size() < k - 1 && current_sum + i < n) {
                current_vector.push_back(i);
                dfs(current_vector, current_sum + i, i, k, n);
                current_vector.pop_back();
            }
        }
    }
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<int> tmp{};
        dfs(tmp, 0, 0, k, n);
        return res;
    }
};
```