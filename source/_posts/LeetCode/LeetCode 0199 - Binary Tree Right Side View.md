---
title: LeetCode 0199 - Binary Tree Right Side View
date: 2018-06-12 11:42:52
categories: LeetCode
---
# Binary Tree Right Side View

<!--more-->

## Desicription

Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

**Example**:

```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

## Solution

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
    void fun(vector<int>& res, int level, TreeNode* root) {
        if(root == NULL)
            return ;
        if(res.size() < level)
            res.push_back(root->val);
        fun(res, level+1, root->right);
        fun(res, level+1, root->left);
    }
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        fun(res, 1, root);
        return res;
    }
};
```