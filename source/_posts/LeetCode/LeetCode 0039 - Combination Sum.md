---
title: LeetCode 0039 - Combination Sum
date: 2017-11-11 15:50:35
categories: LeetCode
---
# Combination Sum #

<!--more-->

## Desicription ##

Given a **set** of candidate numbers (***C***) **(without duplicates)** and a target number (***T***), find all unique combinations in ***C*** where the candidate numbers sums to ***T***.

The **same** repeated number may be chosen from ***C*** unlimited number of times.

**Note:**

- All numbers (including target) will be positive integers.
- The solution set must not contain duplicate combinations.

For example, given candidate set `[2, 3, 6, 7]` and target `7`, 

A solution set is: 

```
[
  [7],
  [2, 2, 3]
]
```

## Solution ##

```cpp
class Solution {
private:
    vector<int> tmp;
    vector<vector<int> > res;
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        dfs(candidates, target, 0);
        return res;
    }

    void dfs(vector<int>& candidates, int target, int index){
        if(index == candidates.size())
            return ;
        int sum = 0;
        for(int i = 0; i < tmp.size(); i++)
            sum += tmp[i];
        if(sum == target){
            res.push_back(tmp);
            return ;
        }
        else if(sum > target)
            return ;
        else{
            for(int i = index; i < candidates.size(); i++){
                tmp.push_back(candidates[i]);
                dfs(candidates, target, i);
                tmp.pop_back();
            }
        }
    }
};
```