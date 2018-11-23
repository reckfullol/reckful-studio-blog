---
title: Sword To Offer 025 - 复杂链表的复制
date: 2018-03-23 17:41:15
categories: Sword To Offer
---
# 复杂链表的复制

<!--more-->

## Desicription

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

## Solution

```cpp
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead) {
        if(pHead == nullptr) {
            return nullptr;
        }
        for(RandomListNode* now = pHead; now != nullptr; now = now->next) {
            auto newNode = new RandomListNode(now->label);
            newNode->next = now->random;
            now->random = newNode;
        }
        for(RandomListNode* now = pHead; now != nullptr; now = now->next) {
            auto newNode = now->random;
            newNode->random = newNode->next != nullptr ? newNode->next->random : nullptr;
        }
        auto newHead = pHead->random;
        for(RandomListNode* now = pHead; now != nullptr; now = now->next) {
            auto newNode = now->random;
            now->random = newNode->next;
            newNode->next = now->next != nullptr ? now->next->random : nullptr;
        }
        return newHead;
    }
};
```