---
title: Sword To Offer 057 - 二叉树的下一个结点
date: 2018-05-13 16:33:51
categories: Sword To Offer
---
# 二叉树的下一个结点

<!--more-->

## Desicription

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

## Solution

```cpp
/*
struct TreeLinkNode {
    int val;
    struct TreeLinkNode *left;
    struct TreeLinkNode *right;
    struct TreeLinkNode *next;
    TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
        
    }
};
*/
class Solution {
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode) {
        if(!pNode) {
            return nullptr;
        }

        if(pNode->right) {
            pNode = pNode->right;
            while(pNode->left) {
                pNode = pNode->left;
            }
            return pNode;
        } else {
            while(pNode->next) {
                if(pNode == pNode->next->left) {
                    return pNode->next;
                } else {
                    pNode = pNode->next;
                }
            }
            return nullptr;
        }
    }
};
```