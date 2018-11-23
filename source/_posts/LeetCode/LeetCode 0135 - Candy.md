---
title: LeetCode 0135 - Candy
date: 2018-05-26 14:45:27
categories: LeetCode
---
# Candy

<!--more-->

## Desicription

There are *N* children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

What is the minimum candies you must give?

**Example** 1:

```
Input: [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```

**Example** 2:

```
Input: [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
             The third child gets 1 candy because it satisfies the above two conditions.
```

## Solution

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candy(ratings.size(), 1);
        for(int i = 1; i < ratings.size(); i++)
            if(ratings[i] > ratings[i-1])
                candy[i] = candy[i-1] + 1;
        for(int i = ratings.size()-1; i >= 1; i--)
            if(ratings[i-1] > ratings[i])
                candy[i-1] = max(candy[i-1], candy[i] + 1);
        int res = 0;
        for(auto it : candy)
            res += it;
        return res;
    }
};
```