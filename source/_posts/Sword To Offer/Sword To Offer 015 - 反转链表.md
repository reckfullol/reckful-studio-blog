---
title: Sword To Offer 015 - 反转链表
date: 2018-03-16 11:26:31
categories: Sword To Offer
---
# 反转链表

<!--more-->

## Desicription

输入一个链表，反转链表后，输出链表的所有元素。

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
    ListNode* ReverseList(ListNode* pHead) {
        ListNode* pre = nullptr;
        ListNode* next = nullptr;
        while(pHead) {
            next = pHead->next;
            pHead->next = pre;
            pre = pHead;
            pHead = next;
        }
        return pre;
    }
};
```