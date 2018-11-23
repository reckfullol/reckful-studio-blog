---
title: LeetCode 0021 - Merge Two Sorted Lists
date: 2017-11-09 20:23:44
categories: LeetCode
---
# Merge Two Sorted Lists #

<!--more-->

## Desicription ##

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(!l1 && !l2)
            return NULL;
        ListNode* head = new ListNode(0);
        ListNode* travel = head;
        while(l1 || l2){
            travel->next = new ListNode(0);
            if(!l1 || (l1 && l2 && l2->val <= l1->val)){
                travel->val = l2->val;
                l2 = l2->next;
                if(!l1 && !l2)
                    travel->next = NULL;
                travel = travel->next;
            }
            else if(!l2 || (l1 && l2 && l2->val > l1->val)){
                travel->val = l1->val;
                l1 = l1->next; 
                if(!l1 && !l2)
                    travel->next = NULL;
                travel = travel->next;
            } 
        }
        return head;
    }
};
```