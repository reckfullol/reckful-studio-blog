---
title: Evaluate Division
date: 2019-09-30 22:23:08
categories: LeetCode
---
# Evaluate Division

<!--more-->

## Desicription

Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.

**Note:**

The array size can be very large. Solution that uses too much extra space will not pass the judge.

**Example:**

```
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(3);

// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);
```

## Solution

```cpp
class Solution {
private:
    std::unordered_map<std::string, std::string> father = std::unordered_map<std::string, std::string>{};
    std::unordered_map<std::string, double> value = std::unordered_map<std::string, double>{};
    std::unordered_map<std::string, std::set<std::pair<std::string, double>>> pairs = std::unordered_map<std::string, std::set<std::pair<std::string, double>>>{};

    inline std::string find(const std::string& a) {
        return father[a] == a ? a : father[a] = find(father[a]);
    }
public:
    std::vector<double> calcEquation(const std::vector<std::vector<std::string>>& equations, const std::vector<double>& values, const std::vector<std::vector<std::string>>& queries) {
        for(unsigned long long i = 0; i < equations.size(); i++) {
            father[equations[i][0]] = equations[i][0];
            father[equations[i][1]] = equations[i][1];
            pairs[equations[i][0]].insert({equations[i][1], values[i]});
            pairs[equations[i][1]].insert({equations[i][0], 1.0f / values[i]});
        }

        for(unsigned long long i = 0; i < equations.size(); i++) {
            auto a = find(equations[i][0]);
            auto b = find(equations[i][1]);
            if(a != b) {
                father[a] = b;
            }
        }

        std::queue<std::string> queue = std::queue<std::string>{};
        for(const auto& pairIt : pairs) {
            if(value.find(pairIt.first) != value.end()) {
                continue;
            }
            value[pairIt.first] = 1.0f;
            queue.push(value.begin()->first);
            while(!queue.empty()) {
                std::string current = queue.front();
                queue.pop();
                for(const auto& it : pairs[current]) {
                    if(value.find(it.first) != value.end()) {
                        continue;
                    } else {
                        value[it.first] = value[current] / it.second;
                        queue.push(it.first);
                    }
                }
            }
        }

        std::vector<double> res = std::vector<double>{};
        for(const auto& it : queries) {
            auto itA = value.find(it[0]);
            auto itB = value.find(it[1]);
            if(itA != value.end() && itB != value.end() && find(itA->first) == find(itB->first)) {
                res.push_back(itA->second / itB -> second);
            } else {
                res.push_back(-1.0f);
            }
        }

        return res;
    }
};
```