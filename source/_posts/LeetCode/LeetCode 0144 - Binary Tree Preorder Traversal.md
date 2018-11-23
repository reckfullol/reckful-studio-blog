---
title: LeetCode 0144 - Binary Tree Preorder Traversal
date: 2018-05-29 14:16:17
categories: LeetCode
---
# Binary Tree Preorder Traversal

<!--more-->

## Desicription

Given a binary tree, return the *preorder* traversal of its nodes' values.

**Example**:

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
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
        vec.push_back(cur->val);
        traversal(cur->left, vec);
        traversal(cur->right, vec);
    }
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> vec;
        traversal(root, vec);
        return vec;
    }
};
```