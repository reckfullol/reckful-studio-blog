---
title: LeetCode 0094 - Binary Tree Inorder Traversal
date: 2017-12-15 11:45:23
categories: LeetCode
---
# Binary Tree Inorder Traversal #

<!--more-->

## Desicription ##

Given a binary tree, return the inorder traversal of its nodes' values.

For example:
Given binary tree `[1,null,2,3]`,

```
   1
    \
     2
    /
   3
```

return `[1,3,2]`.

**Note:** Recursive solution is trivial, could you do it iteratively?

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
public:
    vector<int> res; 
    vector<int> inorderTraversal(TreeNode* root) {
        if(!root)
            return res;
        Traversal(root->left);
        res.push_back(root->val);
        Traversal(root->right);
        return res;
    }
    void Traversal(TreeNode* cur) {
        if(!cur)
            return ;
        Traversal(cur->left);
        res.push_back(cur->val);
        Traversal(cur->right);
    }
};
```