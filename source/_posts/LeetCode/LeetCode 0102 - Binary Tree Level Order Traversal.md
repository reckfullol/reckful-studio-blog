---
title: LeetCode 0102 - Binary Tree Level Order Traversal
date: 2017-12-17 15:54:30
categories: LeetCode
---
# Binary Tree Level Order Traversal #

<!--more-->

## Desicription ##

Given a binary tree, return the *level* order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
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
    void buildVec(TreeNode* root, int level) {
        if(root == NULL)
            return ;
        if(level == res.size())
            res.push_back(vector<int>());
        res[level].push_back(root->val);
        buildVec(root->left, level+1);
        buildVec(root->right, level+1);
    }
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        buildVec(root, 0);
        return res;
    }
};
```