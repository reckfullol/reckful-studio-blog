---
title: LeetCode 0099 - Recover Binary Search Tree
date: 2017-12-15 14:45:51
categories: LeetCode
---
# Recover Binary Search Tree #

<!--more-->

## Desicription ##

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

**Note:**
A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?

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
    TreeNode* pre = new TreeNode(INT_MIN);
    TreeNode* first = NULL;
    TreeNode* second = NULL;

    void searchTree(TreeNode* root) {
        if(!root)
            return ;
        searchTree(root->left);
        if(!first && pre->val >= root->val)
            first = pre;
        if(first && pre->val >= root->val)
            second = root;
        pre = root;
        searchTree(root->right);
    }
public:
    void recoverTree(TreeNode* root) {
        searchTree(root);
        swap(first->val, second->val);
    }
};
```