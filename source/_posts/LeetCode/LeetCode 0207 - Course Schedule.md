---
title: LeetCode 0207 - Course Schedule
date: 2018-06-12 12:37:46
categories: LeetCode
---
# Course Schedule

<!--more-->

## Desicription

There are a total of n courses you have to take, labeled from `0` to `n-1`.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite **pairs**, is it possible for you to finish all courses?

**Example** 1:

```
Input: 2, [[1,0]] 
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
```

**Example** 2:

```
Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```

**Note**:

1. The input prerequisites is a graph represented by **a list of edges**, not adjacency matrices. Read more about [how a graph is represented](https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs).
1. You may assume that there are no duplicate edges in the input prerequisites.

## Solution

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<unordered_set<int>> graph(numCourses);
        for(auto it : prerequisites) {
            graph[it.second].insert(it.first);
        }
        vector<int> degrees(numCourses, 0);
        for(auto it : graph) {
            for(auto index : it) {
                degrees[index]++;
            }
        }
        for(int i = 0; i < numCourses; i++) {
            int index = 0;
            for(; index < numCourses; index++) {
                if(degrees[index] == 0) {
                    break;
                }
            }
            if(index == numCourses) {
                return false;
            }
            degrees[index] = -1;
            for(auto it : graph[index]) {
                degrees[it]--;
            }
        }
        return true;
    }
};
```