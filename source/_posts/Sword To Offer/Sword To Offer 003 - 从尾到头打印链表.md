---
title: Sword To Offer 003 - 从尾到头打印链表
date: 2018-03-12 23:49:09
categories: Sword To Offer
---
# 从尾到头打印链表

<!--more-->

## Desicription

输入一个链表，从尾到头打印链表每个节点的值。

## Solution

```cpp
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> res;
        while(head != nullptr) {
            res.push_back(head->val);
            head = head->next;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```