---
title: Sword To Offer 056 - 删除链表中重复的结点
date: 2018-05-12 21:40:30
categories: Sword To Offer
---
# 删除链表中重复的结点

<!--more-->

## Desicription

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

## Solution

```cpp
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead) {
        auto newHead = new ListNode(0);
        newHead->next = pHead;
        pHead = newHead;
        auto current = pHead;
        while(current && current->next) {
            auto nextNode = current->next;
            bool reFlag = false;
            while(nextNode->next && nextNode->val == nextNode->next->val) {
                nextNode = nextNode->next;
                reFlag = true;
            }
            if(reFlag) {
                current->next = nextNode->next;
            } else {
                current = current->next;
            }
        }
        return pHead->next;
    }
};
```