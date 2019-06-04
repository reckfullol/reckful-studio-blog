---
title: LeetCode 0341 - Flatten Nested List Iterator
date: 2019-06-05 01:45:21
categories: LeetCode
---
# Flatten Nested List Iterator

<!--more-->

## Desicription

Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

**Example 1:**

```
Input: [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,1,2,1,1].
```

**Example 2:**

```
Input: [1,[4,[6]]]
Output: [1,4,6]
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,4,6].
```

## Solution

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class NestedIterator {
private:
    std::vector<int> integerVector;
    int index;
public:
    explicit NestedIterator(const std::vector<NestedInteger>& nestedList) {
        index = 1;
        integerVector = std::vector<int>(1);
        std::function<void(const std::vector<NestedInteger>& nestedLis)> dfs = [&](const std::vector<NestedInteger>& roots) {
            for(const auto& root : roots) {
                if(root.isInteger()) {
                    integerVector.push_back(root.getInteger());
                } else {
                    dfs(root.getList());
                }
            }
        };
        dfs(nestedList);
    }

    int next() {
        return integerVector[index++];
    }

    bool hasNext() {
        return index <= integerVector.size() - 1;
    }
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```