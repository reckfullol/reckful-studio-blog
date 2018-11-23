---
title: Sword To Offer 061 - 序列化二叉树
date: 2018-05-14 20:59:29
categories: Sword To Offer
---
# 序列化二叉树

<!--more-->

## Desicription

请实现两个函数，分别用来序列化和反序列化二叉树

## Solution

```cpp
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
class Solution {
private:
    vector<int> buffer;
    void OnSerialize(TreeNode* root) {
        if(!root) {
            buffer.push_back(INT_MIN);
            return ;
        } else {
            buffer.push_back(root->val);
            OnSerialize(root->left);
            OnSerialize(root->right);
        }
    }

    TreeNode* OnDeserialize(int*& index) {
        if(*index == INT_MIN) {
            index++;
            return nullptr;
        } else {
            auto res = new TreeNode(*index);
            index++;
            res->left = OnDeserialize(index);
            res->right = OnDeserialize(index);
            return res;
        }
    }
public:

    char* Serialize(TreeNode* root) {
        OnSerialize(root);
        auto intBuffer = new int[buffer.size()];
        for(int i = 0; i < buffer.size(); i++) {
            intBuffer[i] = buffer[i];
        }
        return (char*) intBuffer;
    }

    TreeNode* Deserialize(char* str) {
        auto intStr = (int*) str;
        return OnDeserialize(intStr);
    }
};
```