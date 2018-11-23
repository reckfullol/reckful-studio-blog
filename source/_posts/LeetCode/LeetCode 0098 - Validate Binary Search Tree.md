---
title: LeetCode 0098 - Validate Binary Search Tree
date: 2017-12-15 14:43:52
categories: LeetCode
---
# Validate Binary Search Tree #

<!--more-->

## Desicription ##

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

```
    2
   / \
  1   3
```

Binary tree `[2,1,3]`, return true.

**Example 2:**

```
    1
   / \
  2   3
```

Binary tree `[1,2,3]`, return false.

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
    bool isValidBST(TreeNode* root) {
        return isValidBST(root, NULL, NULL);
    }
    bool isValidBST(TreeNode* root, TreeNode* minNode, TreeNode* maxNode) {
        if(!root)
            return 1;
        if((minNode && minNode->val >= root->val) || (maxNode && maxNode->val <= root->val))
            return 0;
        return isValidBST(root->left, minNode, root) && isValidBST(root->right, root, maxNode);
    }
};
```