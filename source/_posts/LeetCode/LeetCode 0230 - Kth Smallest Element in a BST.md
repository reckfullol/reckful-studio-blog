---
title: LeetCode 0230 - Kth Smallest Element in a BST
date: 2018-08-17 08:07:15
categories: LeetCode
---
# Kth Smallest Element in a BST

<!--more-->

## Desicription

Given a binary search tree, write a function `kthSmallest` to find the kth smallest element in it.

**Note:** 

You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Example 1:**

```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

**Example 2:**

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

**Follow up:**

What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

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
    int count = 0;
    int result = 0;
    void dfs(TreeNode* root, int k) {
        if(root == nullptr || count > k) {
            return ;
        }
        dfs(root->left, k);
        count++;
        if(k == count) {
            result = root->val;
            return ;
        }
        dfs(root->right, k);        
    }
public:
    int kthSmallest(TreeNode* root, int k) {
        dfs(root, k);
        return result;
    }
};
```