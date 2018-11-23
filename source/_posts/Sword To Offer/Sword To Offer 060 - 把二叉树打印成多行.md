---
title: Sword To Offer 060 - 把二叉树打印成多行
date: 2018-05-14 13:59:44
categories: Sword To Offer
---
# 把二叉树打印成多行

<!--more-->

## Desicription

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

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
};
*/
class Solution {
public:
    vector<vector<int> > Print(TreeNode* pRoot) {
        if(!pRoot) {
            return vector<vector<int>>();
        }
        vector<vector<TreeNode*>> path(1, vector<TreeNode*>());
        path[0].push_back(pRoot);
        int level = 1;
        while(true) {
            path.emplace_back();
            bool addFlag = false;
            for(int i = 0; i < path[level - 1].size(); i++) {
                auto left = path[level - 1][i]->left;
                auto right = path[level - 1][i]->right;
                if(left) {
                    path[level].push_back(left);
                    addFlag = true;
                }
                if(right) {
                    path[level].push_back(right);
                    addFlag = true;
                }
            }
            if(addFlag) {
                level++;
            } else {
                path.pop_back();
                break;
            }
        }
        vector<vector<int>> res(path.size(), vector<int>());
        for(int i = 0; i < path.size(); i++) {
            for (int j = 0; j < path[i].size(); j++) {
                res[i].push_back(path[i][j]->val);
            }
        }
        return res;
    }
};

```