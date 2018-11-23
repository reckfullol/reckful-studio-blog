---
title: LeetCode 0095 - Unique Binary Search Trees II
date: 2017-12-15 14:17:26
categories: LeetCode
---
# Unique Binary Search Trees II #

<!--more-->

## Desicription ##

Given an integer *n*, generate all structurally unique **BST's** (binary search trees) that store values 1...*n*.

For example,
Given *n* = 3, your program should return all 5 unique **BST's** shown below.

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

## Solution ##

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    vector<TreeNode*> searchTree(int left, int right) {
        vector<TreeNode*> res;
        if(left > right) {
            res.push_back(NULL);
            return res;
        }
        for(int i = left; i <= right; i++) {
            vector<TreeNode*> leftVec = searchTree(left, i-1);
            vector<TreeNode*> rightVec = searchTree(i+1, right);
            for(int j = 0; j < leftVec.size(); j++) {
                for(int k = 0; k < rightVec.size(); k++) {
                    TreeNode* root = new TreeNode(i);
                    root->left = leftVec[j];
                    root->right = rightVec[k];
                    res.push_back(root);
                }
            }
        }
        return res;
    }
public:
    vector<TreeNode*> generateTrees(int n) {
        if(!n)
            return vector<TreeNode*>(0);
        return searchTree(1, n);
    }
};
```