---
title: Sword To Offer 036 - 两个链表的第一个公共结点
date: 2018-03-31 17:18:15
categories: Sword To Offer
---
# 两个链表的第一个公共结点

<!--more-->

## Desicription

输入两个链表，找出它们的第一个公共结点。

## Solution

```cpp
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        auto pointTemp = pHead1;
        int length1 = 0;
        int length2 = 0;
        while(pointTemp) {
            pointTemp = pointTemp->next;
            length1++;
        }
        pointTemp = pHead2;
        while(pointTemp) {
            pointTemp = pointTemp->next;
            length2++;
        }
        while(true) {
            if(length1 > length2) {
                pHead1 = pHead1->next;
                length1--;
            } else if(length1 < length2) {
                pHead2 = pHead2->next;
                length2--;
            } else {
                if(pHead1 == pHead2) {
                    return pHead1;
                } else {
                    pHead1 = pHead1->next;
                    pHead2 = pHead2->next;
                    length1--;
                    length2--;
                }
            }
        }
    }
};
```