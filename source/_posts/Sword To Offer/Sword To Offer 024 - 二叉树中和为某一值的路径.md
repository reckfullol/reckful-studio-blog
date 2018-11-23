---
title: Sword To Offer 024 - 二叉树中和为某一值的路径
date: 2018-03-23 14:26:30
categories: Sword To Offer
---
# 二叉树中和为某一值的路径

<!--more-->

## Desicription

输入一颗二叉树和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

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
vector<vector<int>> res;
vector<int> tmp;
private:
    void Dfs(TreeNode* root, int expectNumber) {
        if(root == nullptr) {
            return ;
        }
        tmp.push_back(root->val);
        expectNumber -= root->val;
        if(expectNumber == 0 && root->left == nullptr && root->right == nullptr) {
            res.push_back(tmp);
        }
        Dfs(root->left, expectNumber);
        Dfs(root->right, expectNumber);
        if(!tmp.empty()) {
            tmp.pop_back();
        }
    }
public:
    vector<vector<int>> FindPath(TreeNode* root,int expectNumber) {
        Dfs(root, expectNumber);
        return res;
    }
};
```