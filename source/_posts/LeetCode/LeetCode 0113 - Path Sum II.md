---
title: LeetCode 0113 - Path Sum II
date: 2017-12-17 18:45:20
categories: LeetCode
---
# Path Sum II #

<!--more-->

## Desicription ##

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and `sum = 22`,

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

return

```
[
   [5,4,11,2],
   [5,8,4,5]
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
    void searchTree(TreeNode* root, int sum, const int& target, vector<int> vec) {
        if(root == NULL)
            return ;
        vec.push_back(root->val);
        if(root->left == NULL && root->right == NULL)
            if(sum + root->val == target)
                res.push_back(vec);
        searchTree(root->left, sum + root->val, target, vec);
        searchTree(root->right, sum + root->val, target, vec);
    }
public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if(root == NULL)
            return res;
        searchTree(root, 0, sum, vector<int>());
        return res;
    }
};
```