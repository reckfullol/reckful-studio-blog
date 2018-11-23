---
title: LeetCode 0002 - Add Two Numbers
date: 2017-11-07 21:14:21
categories: LeetCode
---
# Add Two Numbers #

<!--more-->

## Desicription ##

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Input:** (2 -> 4 -> 3) + (5 -> 6 -> 4)

**Output:** 7 -> 0 -> 8

## Solution ##

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head = new ListNode(0);
        ListNode *p = head;
        int sum = 0;
        while(l1 || l2){
            if(p->next)
                p = p->next;
            if(l1)
                sum += l1->val, l1 = l1->next;
            if(l2)
                sum += l2->val, l2 = l2->next;
            p->val = sum % 10;
            p->next = new ListNode(0);
            sum /= 10;
        }
        if(sum)
            p->next->val = sum;
        else
            p->next = NULL;
        return head;
    }
};
```