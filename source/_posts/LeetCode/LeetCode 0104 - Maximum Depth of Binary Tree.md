---
title: LeetCode 0104 - Maximum Depth of Binary Tree
date: 2017-12-17 16:25:35
categories: LeetCode
---
# Maximum Depth of Binary Tree #

<!--more-->

## Desicription ##

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

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
    int res = 0;
    void findMaxDepth(TreeNode* root, int level) {
        if(root == NULL)
            return ;
        res = max(res, level);
        findMaxDepth(root->left, level+1);
        findMaxDepth(root->right, level+1);
    }
public:
    int maxDepth(TreeNode* root) {
        findMaxDepth(root, 1);
        return res;
    }
};
```