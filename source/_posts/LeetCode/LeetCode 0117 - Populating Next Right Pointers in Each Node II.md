---
title: LeetCode 0117 - Populating Next Right Pointers in Each Node II
date: 2017-12-18 13:13:23
categories: LeetCode
---
# Populating Next Right Pointers in Each Node II #

<!--more-->

## Desicription ##

Follow up for problem "`Populating Next Right Pointers in Each Node`".

What if the given tree could be any binary tree? Would your previous solution still work?

**Note:**

- You may only use constant extra space.

For example,
Given the following binary tree,

```
         1
       /  \
      2    3
     / \    \
    4   5    7
```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```

## Solution ##

```cpp
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        TreeLinkNode* cur = root;
        TreeLinkNode* nextLevelHead = NULL;
        TreeLinkNode* nextLevelPre = NULL;
        while(cur != NULL) {
            while(cur != NULL) {
                if(cur->left != NULL) {
                    if(nextLevelPre != NULL)
                        nextLevelPre->next = cur->left;
                    else
                        nextLevelHead = cur->left;
                    nextLevelPre = cur->left;
                }
                if(cur->right != NULL) {
                    if(nextLevelPre != NULL)
                        nextLevelPre->next = cur->right;
                    else
                        nextLevelHead = cur->right;
                    nextLevelPre = cur->right;
                }
                cur = cur->next;
            }
            cur = nextLevelHead;
            nextLevelHead = NULL;
            nextLevelPre = NULL;
        }        
    }
};
```