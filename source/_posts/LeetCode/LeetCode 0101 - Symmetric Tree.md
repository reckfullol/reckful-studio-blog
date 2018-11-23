---
title: LeetCode 0101 - Symmetric Tree
date: 2017-12-17 15:03:17
categories: LeetCode
---
# Symmetric Tree #

<!--more-->

## Desicription ##

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following `[1,2,2,null,3,null,3]` is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

**Note:**
Bonus points if you could solve it both recursively and iteratively.

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
    bool isSymmetric(TreeNode* left, TreeNode* right) {
        if(left == NULL && right == NULL)
            return 1;
        if(left == NULL || right == NULL)
            return 0;
        if(left->val == right->val)
            return isSymmetric(left->left, right->right) && isSymmetric(left->right, right->left);
        return 0;
    }
public:
    bool isSymmetric(TreeNode* root) {
        if(root == NULL)
            return 1;
        return isSymmetric(root->left, root->right);
    }
};
```