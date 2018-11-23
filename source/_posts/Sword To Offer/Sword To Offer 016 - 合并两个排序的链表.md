---
title: Sword To Offer 016 - 合并两个排序的链表
date: 2018-03-16 11:37:57
categories: Sword To Offer
---
# 合并两个排序的链表

<!--more-->

## Desicription

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

## Solution

```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2) {
        ListNode* head = nullptr;
        ListNode* current = nullptr;
        while(pHead1 || pHead2) {
            if(head == nullptr) {
                if(pHead1 == nullptr) {
                    head = pHead2;
                    pHead2 = pHead2->next;
                } else if(pHead2 == nullptr) {
                    head = pHead1;
                    pHead1 = pHead1->next;
                } else {
                    if(pHead1->val <= pHead2->val) {
                        head = pHead1;
                        pHead1 = pHead1->next;
                    } else {
                        head = pHead2;
                        pHead2 = pHead2->next;
                    }
                }
                current = head;
            } else {
                if(pHead1 == nullptr) {
                    current->next = pHead2;
                    pHead2 = pHead2->next;
                } else if(pHead2 == nullptr) {
                    current->next = pHead1;
                    pHead1 = pHead1->next;
                } else {
                    if(pHead1->val <= pHead2->val) {
                        current->next = pHead1;
                        pHead1 = pHead1->next;
                    } else {
                        current->next = pHead2;
                        pHead2 = pHead2->next;
                    }
                }
                current = current->next;
            }
        }
        return head;
    }
};
```