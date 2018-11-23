---
title: Sword To Offer 039 - 平衡二叉树
date: 2018-03-31 20:54:21
categories: Sword To Offer
---
# 平衡二叉树

<!--more-->

## Desicription

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

## Solution

```cpp
class Solution {
private:
    int GetHeight(TreeNode* root) {
        if(root == nullptr) {
            return 0;
        }
        int leftHeight = GetHeight(root->left);
        int rightHeight = GetHeight(root->right);
        return max(leftHeight, rightHeight) + 1;
    }
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        if(pRoot == nullptr) {
            return true;
        }
        int leftHeight = GetHeight(pRoot->left);
        int rightHeight = GetHeight(pRoot->right);
        if(abs(leftHeight - rightHeight) > 1) {
            return false;
        }
        return IsBalanced_Solution(pRoot->left) && IsBalanced_Solution(pRoot->right);
    }
};
```