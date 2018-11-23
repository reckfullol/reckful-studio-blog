---
title: Sword To Offer 059 - 按之字形顺序打印二叉树
date: 2018-05-14 13:57:42
categories: Sword To Offer
---
# 按之字形顺序打印二叉树

<!--more-->

## Desicription

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

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
            if(i & 1) {
                for(int j = path[i].size() - 1; j >= 0; j--) {
                    res[i].push_back(path[i][j]->val);
                }
            } else {
                for (int j = 0; j < path[i].size(); j++) {
                    res[i].push_back(path[i][j]->val);
                }
            }
        }
        return res;
    }
};
```