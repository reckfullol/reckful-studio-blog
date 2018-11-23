---
title: LeetCode 0040 - Combination Sum II
date: 2017-11-11 16:00:27
categories: LeetCode
---
# Combination Sum II #

<!--more-->

## Desicription ##

Given a collection of candidate numbers (***C***) and a target number (***T***), find all unique combinations in ***C*** where the candidate numbers sums to ***T***.

Each number in ***C*** may only be used **once** in the combination.

**Note:**

- All numbers (including target) will be positive integers.
- The solution set must not contain duplicate combinations.

For example, given candidate set `[10, 1, 2, 7, 6, 1, 5]` and target `8`, 

A solution set is: 

```
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

## Solution ##

```cpp
class Solution {
private:
    vector<int> tmp;
    vector<vector<int>> res;
    set<vector<int>> S;
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        dfs(candidates, target, 0);
        for(auto it = S.begin(); it != S.end(); it++)
            res.push_back(*it);
        return res;
    }

    void dfs(vector<int>& candidates, int target, int index){
        int sum = 0;
        for(int i = 0; i < tmp.size(); i++)
            sum += tmp[i];
        if(sum == target){
            S.insert(tmp);
            return ;
        }
        else if(sum > target)
            return ;
        else{
            for(int i = index; i < candidates.size(); i++){
                tmp.push_back(candidates[i]);
                dfs(candidates, target, i+1);
                tmp.pop_back();
            }
        }
    }
};
```