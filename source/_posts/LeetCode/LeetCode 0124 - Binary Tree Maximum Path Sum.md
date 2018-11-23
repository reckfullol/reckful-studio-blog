---
title: LeetCode 0124 - Binary Tree Maximum Path Sum
date: 2017-12-18 15:14:41
categories: LeetCode
---
# Binary Tree Maximum Path Sum #

<!--more-->

## Desicription ##

Given a binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.

For example:
Given the below binary tree,

```
       1
      / \
     2   3
```

Return `6`.

## Solution ##

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    int res = INT_MIN;
    int getSonMax(TreeNode* cur) {
        if(cur == NULL)
            return 0;
        int l = max(0, getSonMax(cur->left));
        int r = max(0, getSonMax(cur->right));
        res = max(res, l + r + cur->val);
        return cur->val + max(l, r);
    }
public:
    int maxPathSum(TreeNode* root) {
        getSonMax(root);
        return res;
    }
};
```