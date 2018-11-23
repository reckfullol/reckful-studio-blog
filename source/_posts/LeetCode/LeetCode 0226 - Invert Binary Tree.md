---
title: LeetCode 0226 - Invert Binary Tree
date: 2018-07-05 16:26:59
categories: LeetCode
---
# Invert Binary Tree

<!--more-->

## Desicription

Invert a binary tree.

**Example:**

Input:

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

Output:

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**Trivia:**

This problem was inspired by [this original tweet](https://twitter.com/mxcl/status/608682016205344768) by [Max Howell](https://twitter.com/mxcl/status/608682016205344768):

> Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.

## Solution

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
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr) {
            return nullptr;
        }
        auto left_node = invertTree(root->left);
        auto right_node = invertTree(root->right);
        root->left = right_node;
        root->right = left_node;
        return root;
    }
};
```