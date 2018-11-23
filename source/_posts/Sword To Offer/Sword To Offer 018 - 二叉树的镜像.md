---
title: Sword To Offer 018 - 二叉树的镜像
date: 2018-03-16 15:25:02
categories: Sword To Offer
---
# 二叉树的镜像

<!--more-->

## Desicription

操作给定的二叉树，将其变换为源二叉树的镜像。

二叉树的镜像定义：源二叉树 

```
    	    8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11
    	镜像二叉树
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11 9 7  5
```

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
    void Modify(TreeNode* pRoot) {
        if(!pRoot) {
            return ;
        }
        TreeNode* tempNode = pRoot->left;
        pRoot->left = pRoot->right;
        pRoot->right = tempNode;
        Modify(pRoot->left);
        Modify(pRoot->right);
    }
public:
    void Mirror(TreeNode *pRoot) {
        Modify(pRoot);
    }
};
```