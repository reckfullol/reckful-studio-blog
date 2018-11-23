---
title: Sword To Offer 062 - 二叉搜索树的第k个结点
date: 2018-05-14 21:24:58
categories: Sword To Offer
---
# 二叉搜索树的第k个结点

<!--more-->

## Desicription

给定一颗二叉搜索树，请找出其中的第k大的结点。按结点数值大小顺序第三个结点的值为4。

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
private:
    TreeNode* res = nullptr;
    int currentCount = 0;
    bool foundFLag = false;
    void OnSearch(TreeNode* pRoot, int k) {
        if(!pRoot || foundFLag) {
            return ;
        }
        OnSearch(pRoot->left, k);
        currentCount++;
        if(currentCount == k) {
            res = pRoot;
            foundFLag = true;
            return ;
        }
        OnSearch(pRoot->right, k);
    }
public:
    TreeNode* KthNode(TreeNode* pRoot, int k) {
        OnSearch(pRoot, k);
        return res;
    }
};
```