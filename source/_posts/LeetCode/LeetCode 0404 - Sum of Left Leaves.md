---
title: LeetCode 0404 - Sum of Left Leaves
date: 2019-10-12 20:11:32
categories: LeetCode
---
# Sum of Left Leaves

<!--more-->

## Desicription

Find the sum of all left leaves in a given binary tree.

**Example:**

```
    3
   / \
  9  20
    /  \
   15   7
```

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.

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
    void dfs(TreeNode* root, bool isLeft, int* res) {
        if(root == nullptr) {
            return ;
        }
        if(root->left == nullptr && root->right == nullptr && isLeft) {
            *res += root->val;
        }
        dfs(root->left, true, res);
        dfs(root->right, false, res);
    }
public:
    int sumOfLeftLeaves(TreeNode* root) {
        int res = 0;
        dfs(root, false, &res);
        return res;
    }
};
```