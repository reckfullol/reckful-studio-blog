---
title: LeetCode 0111 - Minimum Depth of Binary Tree
date: 2017-12-17 18:40:50
categories: LeetCode
---
# Minimum Depth of Binary Tree #

<!--more-->

## Desicription ##

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

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
    int res = INT_MAX;
    void searchTree(TreeNode* root, int depth) {
        if(root == NULL)
            return ;
        if(root->left == NULL && root->right == NULL)
            res = min(res, depth);
        searchTree(root->left, depth+1);
        searchTree(root->right, depth+1);    
    }
public:
    int minDepth(TreeNode* root) {
        if(root == NULL)
            return 0;
        searchTree(root, 1);
        return res;
    }
};
```