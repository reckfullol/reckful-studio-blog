---
title: Sword To Offer 022 - 从上往下打印二叉树
date: 2018-03-22 20:13:30
categories: Sword To Offer
---
# 从上往下打印二叉树

<!--more-->

## Desicription

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

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
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> res;
        queue<TreeNode*> bfs;
        if(root == nullptr) {
            return res;
        }
        bfs.push(root);
        while(!bfs.empty()) {
            root = bfs.front();
            bfs.pop();
            if(root->left) {
                bfs.push(root->left);
            }
            if(root->right) {
                bfs.push(root->right);
            }
            res.push_back(root->val);
        }
        return res;
    }
};
```