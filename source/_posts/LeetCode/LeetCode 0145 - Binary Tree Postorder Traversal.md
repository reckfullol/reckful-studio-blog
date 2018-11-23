---
title: LeetCode 0145 - Binary Tree Postorder Traversal
date: 2018-05-29 14:36:18
categories: LeetCode
---
# Binary Tree Postorder Traversal

<!--more-->

## Desicription

Given a binary tree, return the *postorder* traversal of its nodes' values.

**Example**:

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [3,2,1]
```

**Follow up**: Recursive solution is trivial, could you do it iteratively?

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
    void traversal(TreeNode* cur, vector<int>& vec) {
        if(cur == NULL)
            return ;
        traversal(cur->left, vec);
        traversal(cur->right, vec);
        vec.push_back(cur->val);
    }
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> vec;
        traversal(root, vec);
        return vec;
    }
};
```