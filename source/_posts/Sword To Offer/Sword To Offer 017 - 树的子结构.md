---
title: Sword To Offer 017 - 树的子结构
date: 2018-03-16 15:04:07
categories: Sword To Offer
---
# 树的子结构

<!--more-->

## Desicription

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

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
    bool IsSubtree(TreeNode* pRoot1, TreeNode* pRoot2) {
        if(pRoot2 == nullptr) {
            return true;
        } else if(pRoot1 == nullptr) {
            return false;
        } else if(pRoot1->val != pRoot2->val) {
            return false;
        }
        return IsSubtree(pRoot1->left, pRoot2->left) && IsSubtree(pRoot1->right, pRoot2->right);
    }
public:
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2) {
        if(pRoot1 == nullptr || pRoot2 == nullptr) {
            return false;
        }
        bool result = false;
        if(pRoot1->val == pRoot2->val) {
            result = IsSubtree(pRoot1, pRoot2);
        }
        if(!result) {
            result = IsSubtree(pRoot1->left, pRoot2);
        }
        if(!result) {
            result = IsSubtree(pRoot1->right, pRoot2);
        }
        return result;
    }
};
```