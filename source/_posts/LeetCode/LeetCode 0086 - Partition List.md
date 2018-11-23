---
title: LeetCode 0086 - Partition List
date: 2017-11-21 09:27:58
categories: LeetCode
---
# Partition List #

<!--more-->

## Desicription ##

Given a linked list and a value *x*, partition it such that all nodes less than *x* come before nodes greater than or equal to *x*.

You should preserve the original relative order of the nodes in each of the two partitions.

For example,
Given `1->4->3->2->5->2` and *x* = 3,
return `1->2->2->4->3->5`.

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
    ListNode* partition(ListNode* head, int x) {
        ListNode* low = new ListNode(0);
        ListNode* high = new ListNode(0);
        ListNode* base_low = low;
        ListNode* base_high = high;
        while(head){
            if(head->val < x)
                low->next = head, low = low->next;
            else
                high->next = head, high = high->next;
            head = head->next; 
        }
        high->next = NULL;
        low->next = base_high->next;
        return base_low->next;
    }
};
```