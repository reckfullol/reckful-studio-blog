---
title: LeetCode 0108 - Convert Sorted Array to Binary Search Tree
date: 2017-12-17 16:39:09
categories: LeetCode
---
# Convert Sorted Array to Binary Search Tree #

<!--more-->

## Desicription ##

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

**Example:**

```
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
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
    TreeNode* buildTree(int left, int right, vector<int>& nums) {
        if(left > right)
            return 0;
        int mid = (left + right)>>1;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = buildTree(left, mid-1, nums);
        root->right = buildTree(mid+1, right, nums);
        return root;
    }
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return buildTree(0, nums.size()-1, nums);
    }
};
```