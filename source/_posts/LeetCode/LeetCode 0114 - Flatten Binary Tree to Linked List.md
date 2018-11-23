---
title: LeetCode 0114 - Flatten Binary Tree to Linked List
date: 2017-12-17 18:47:07
categories: LeetCode
---
# Flatten Binary Tree to Linked List #

<!--more-->

## Desicription ##

Given a binary tree, flatten it to a linked list in-place.

For example,
Given

```
         1
        / \
       2   5
      / \   \
     3   4   6
```

The flattened tree should look like:

```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

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
    TreeNode* prev = NULL;
public:
    void flatten(TreeNode* root) {
        if(root == NULL)
            return ;
        flatten(root->right);
        flatten(root->left);
        root->right = prev;
        root->left = NULL;
        prev = root;
    }
};
```