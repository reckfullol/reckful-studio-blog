---
title: LeetCode 0337 - House Robber III
date: 2019-06-05 00:58:14
categories: LeetCode
---
# House Robber III

<!--more-->

## Desicription

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

**Example 1:**

```
Input: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

Output: 7 
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

**Example 2:**

```
Input: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
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
public:
    int rob(TreeNode* root) {
        std::function<int(TreeNode*, int&, int&)> dfs = [&](TreeNode* root, int& left, int& right) {
            if(root == nullptr) {
                return 0;
            }

            int leftLeft = 0;
            int leftRight = 0;
            int rightLeft = 0;
            int rightRight = 0;

            left = dfs(root->left, leftLeft, leftRight);
            right = dfs(root->right, rightLeft, rightRight);

            return std::max(root->val + leftLeft + leftRight + rightLeft + rightRight, left + right);
        };

        int left = 0;
        int right = 0;
        return dfs(root, left, right);
    }
};
```