---
title: Sword To Offer 014 - 链表中倒数第k个结点
date: 2018-03-16 10:44:05
categories: Sword To Offer
---
# 链表中倒数第k个结点

<!--more-->

## Desicription

输入一个链表，输出该链表中倒数第k个结点。

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        int count = 0;
        ListNode* currentNode= pListHead;
        while(currentNode) {
            count++;
            currentNode = currentNode->next;
        }
        count -= k;
        currentNode = pListHead;
        while(currentNode) {
            if(!count) {
                return currentNode;
            }
            count--;
            currentNode = currentNode->next;
        }
        return nullptr;
    }
};
```