---
title: Sword To Offer 058 - 对称的二叉树
date: 2018-05-14 10:55:18
categories: Sword To Offer
---
# 对称的二叉树

<!--more-->

## Desicription

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

## Solution

```cpp
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};

class Solution {
private:
    bool isSymmetrical(TreeNode* left, TreeNode* right) {
        if(left == nullptr && right == nullptr) {
            return true;
        } else if(left == nullptr || right == nullptr) {
            return false;
        } else {
            if(left->val == right->val) {
                return isSymmetrical(left->left, right->right) && isSymmetrical(left->right, right->left);
            } else {
                return false;
            }
        }
    }
public:
    bool isSymmetrical(TreeNode* pRoot) {
        if(!pRoot) {
            return true;
        } else {
            return isSymmetrical(pRoot->left, pRoot->right);
        }
    }
};
```