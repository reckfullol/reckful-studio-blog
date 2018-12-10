---
title: LeetCode 0257 - Binary Tree Paths
date: 2018-12-10 09:37:46
categories: LeetCode
---
# Binary Tree Paths

<!--more-->

## Desicription

Given a binary tree, return all root-to-leaf paths.

**Note**: A leaf is a node with no children.

**Example**:

```
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3
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
    vector<string> res{};
    void search(TreeNode* root, string path) {
        if(root == nullptr) {
            return ;
        }
        path += path == "" ? to_string(root->val) : "->" + to_string(root->val);
        if(root->left == nullptr && root->right == nullptr) {
            res.push_back(path);
            return ;
        }
        if(root->left) {
            search(root->left, path);
        }
        if(root->right) {
            search(root->right, path);
        }
    }
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        search(root, "");
        return res;
    }
};
```