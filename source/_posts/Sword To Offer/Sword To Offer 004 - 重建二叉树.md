---
title: Sword To Offer 004 - 重建二叉树
date: 2018-03-13 21:56:42
categories: Sword To Offer
---
# 重建二叉树

<!--more-->

## Desicription

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

## Solution

```cpp
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    TreeNode* Handle(int midOrderLeft, int midOrderRight, int preOrderLeft, TreeNode* root, const vector<int>& pre, const vector<int>& mid) {
        for(int index = midOrderLeft; index <= midOrderRight; index++) {
            if(mid[index] == pre[preOrderLeft]) {
                root = new TreeNode(mid[index]);
                root->left = Handle(midOrderLeft, index - 1, preOrderLeft + 1, root->left, pre, mid);
                root->right = Handle(index + 1, midOrderRight, index - midOrderLeft + preOrderLeft + 1, root->right, pre, mid);
                return root;
            }
        }
        return nullptr;
    }

public:
    TreeNode* reConstructBinaryTree(const vector<int>& pre, const vector<int>& mid) {
        return Handle(0, mid.size() - 1, 0, nullptr, pre, mid);
    }
};
```