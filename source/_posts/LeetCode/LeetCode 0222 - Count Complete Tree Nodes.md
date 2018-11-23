---
title: LeetCode 0222 - Count Complete Tree Nodes    
date: 2018-07-04 00:50:54
categories: LeetCode
---
# Count Complete Tree Nodes

<!--more-->

## Desicription

Given a **complete** binary tree, count the number of nodes.

**Note**:

In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

**Example**:

```
Input: 
    1
   / \
  2   3
 / \  /
4  5 6
```

## Solution

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        return countAll(root, -1, -1);
    }
    int countAll(TreeNode* root, int left_depth, int right_depth) {
        if(left_depth == -1) {
            left_depth = leftDepth(root);
        }
        if(right_depth == -1) {
            right_depth = rightDepth(root);
        }
        if(left_depth == right_depth) {
            return static_cast<int>(pow(2, left_depth) - 1);
        }
        return 1 + countAll(root->left, left_depth - 1, -1) + countAll(root->right, -1, right_depth - 1);
    }
    int leftDepth(TreeNode* root) {
        if(root == nullptr) {
            return 0;
        }
        return 1 + leftDepth(root->left);
    }
    int rightDepth(TreeNode* root) {
        if(root == nullptr) {
            return 0;
        }
        return 1 + rightDepth(root->right);
    }
};
```