---
title: LeetCode 0105 - Construct Binary Tree from Preorder and Inorder Traversal
date: 2017-12-17 16:28:00
categories: LeetCode
---
# Construct Binary Tree from Preorder and Inorder Traversal #

<!--more-->

## Desicription ##

Given preorder and inorder traversal of a tree, construct the binary tree.

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
    TreeNode* generateTree(int preStart, int inStart, int inEnd, vector<int>& preorder, vector<int>& inorder) {
        if(preStart > preorder.size()-1 || inStart > inEnd)
            return NULL;
        TreeNode* root = new TreeNode(preorder[preStart]);
        int inIndex = 0;
        for(int i = inStart; i <= inEnd; i++) {
            if(inorder[i] == preorder[preStart]){
                inIndex = i;
                break;
            }
        }
        root->left = generateTree(preStart+1, inStart, inIndex-1, preorder, inorder);
        root->right = generateTree(preStart+inIndex-inStart+1, inIndex+1, inEnd, preorder, inorder);
        return root;
    }

public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return generateTree(0, 0, inorder.size()-1, preorder, inorder);
    }
};
```