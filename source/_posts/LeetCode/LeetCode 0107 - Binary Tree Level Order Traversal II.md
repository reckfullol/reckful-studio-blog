---
title: LeetCode 0107 - Binary Tree Level Order Traversal II
date: 2017-12-17 16:31:28
categories: LeetCode
---
# Binary Tree Level Order Traversal II #

<!--more-->

## Desicription ##

Given a binary tree, return the *bottom-up level* order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]
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
    vector<vector<int>> res;
    void searchTree(TreeNode* root, int level) {
        if(!root)
            return ;
        if(level == res.size())
            res.push_back(vector<int>());
        res[level].push_back(root->val);
        searchTree(root->left, level+1);
        searchTree(root->right, level+1);
    }
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        searchTree(root, 0);
        for(int i = 0, j = res.size()-1; i <= j; i++, j--)
            swap(res[i], res[j]);
        return res;
    }
};
```