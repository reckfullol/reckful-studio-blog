---
title: LeetCode 0106 - Construct Binary Tree from Inorder and Postorder Traversal
date: 2017-12-17 16:30:11
categories: LeetCode
---
# Construct Binary Tree from Inorder and Postorder Traversal #

<!--more-->

## Desicription ##

Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

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
    TreeNode* generateTree(int postStart, int postEnd, int inStart, int inEnd, vector<int>& postorder, vector<int>& inorder) {
        if(postStart > postEnd || inStart > inEnd)
            return NULL;
        TreeNode* root = new TreeNode(postorder[postEnd]);
        int inIndex = 0;
        for(int i = inStart; i <= inEnd; i++) {
            if(inorder[i] == postorder[postEnd]){
                inIndex = i;
                break;
            }
        }
        root->left = generateTree(postStart, postStart+inIndex-inStart-1, inStart, inIndex-1, postorder, inorder);
        root->right = generateTree(postEnd-inEnd+inIndex, postEnd-1, inIndex+1, inEnd, postorder, inorder);
        return root;
    }

public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return generateTree(0, postorder.size()-1, 0, inorder.size()-1, postorder, inorder);
    }
};
```