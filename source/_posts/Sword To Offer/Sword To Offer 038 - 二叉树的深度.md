---
title: Sword To Offer 038 - 二叉树的深度
date: 2018-03-31 17:31:47
categories: Sword To Offer
---
# 二叉树的深度

<!--more-->

## Desicription

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

## Solution

```cpp
class Solution {
public:
    int TreeDepth(TreeNode* pRoot) {
        int maxLevel = 0;
        queue<pair<TreeNode*, int>> q;
        q.push(make_pair(pRoot, 1));
        while(!q.empty()) {
            auto current = q.front();
            q.pop();
            if(current.first == nullptr) {
                continue;
            }
            maxLevel = max(maxLevel, current.second);
            q.push(make_pair(current.first->left, current.second + 1));
            q.push(make_pair(current.first->right, current.second + 1));
        }
        return maxLevel;
    }
};
```