---
title: Sword To Offer 055 - 链表中环的入口结点
date: 2018-05-12 18:53:47
categories: Sword To Offer
---
# 链表中环的入口结点

<!--more-->

## Desicription

一个链表中包含环，请找出该链表的环的入口结点。

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
private:
    unordered_set<ListNode*> book;
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead) {
        while(pHead != nullptr) {
            if(book.find(pHead) == book.end()) {
                book.insert(pHead);
                pHead = pHead->next;
            } else {
                return pHead;
            }
        }
        return pHead;
    }
};
```
