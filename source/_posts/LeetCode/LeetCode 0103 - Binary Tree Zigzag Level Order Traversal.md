---
title: LeetCode 0103 - Binary Tree Zigzag Level Order Traversal
date: 2017-12-17 15:56:48
categories: LeetCode
---
# Binary Tree Zigzag Level Order Traversal #

<!--more-->

## Desicription ##

Given a binary tree, return the *zigzag level* order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```
[
  [3],
  [20,9],
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        buildVec(root, 0);
        for(int i = 0; i < res.size(); i++) {
            if(i & 1) {
                for(int left = 0, right = res[i].size() - 1; left <= right; left++, right--) {
                    swap(res[i][left], res[i][right]);
                }
            }
        }
        return res;
    }
};
```