---
title: Sword To Offer 026 - 二叉搜索树与双向链表
date: 2018-03-27 14:49:17
categories: Sword To Offer
---
# 二叉搜索树与双向链表

<!--more-->

## Desicription

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

## Solution

```cpp
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
private:
    TreeNode* pre = nullptr;
    TreeNode* res = nullptr;
    void Dfs(TreeNode* root) {
        if(root == nullptr) {
            return ;
        }
        Dfs(root->left);
        if(pre == nullptr) {
            pre = root;
            root->left = nullptr;
            res = pre;
        } else {
            root->left = pre;
            pre->right = root;
            pre = root;
        }
        Dfs(root->right);
    }
public:
    TreeNode* Convert(TreeNode* pRootOfTree) {
        Dfs(pRootOfTree);
        return res;
    }
};
```