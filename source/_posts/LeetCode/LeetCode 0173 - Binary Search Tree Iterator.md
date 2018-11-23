---
title: LeetCode 0173 - Binary Search Tree Iterator
date: 2018-06-04 12:13:45
categories: LeetCode
---
# Binary Search Tree Iterator

<!--more-->

## Desicription

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

Note: `next()` and `hasNext()` should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

## Solution

```cpp
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

 /**
 * Your BSTIterator will be called like this:
 * BSTIterator i = BSTIterator(root);
 * while (i.hasNext()) cout << i.next();
 */
class BSTIterator {
private:
    stack<TreeNode* > nodeStack;
    void findLeft(TreeNode* cur) {
        while(cur)
            nodeStack.push(cur), cur = cur->left;
    }
public:
    BSTIterator(TreeNode *root) {
        findLeft(root);
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return nodeStack.size();
    }

    /** @return the next smallest number */
    int next() {
        TreeNode* cur = nodeStack.top();
        nodeStack.pop();
        if(cur->right)
            findLeft(cur->right);
        return cur->val;
    }
};
```