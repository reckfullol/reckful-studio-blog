---
title: LeetCode 0206 - Reverse Linked List
date: 2018-06-12 12:37:46
categories: LeetCode
---
# Reverse Linked List

<!--more-->

## Desicription

Reverse a singly linked list.

**Example**:

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

**Follow up**:

A linked list can be reversed either iteratively or recursively. Could you implement both?

## Solution

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
    ListNode* reverseList(ListNode* head) {
        ListNode* newHead = NULL;
        while(head) {
            ListNode* next = head->next;
            head->next = newHead;
            newHead = head;
            head = next;
        }
        return newHead;
    }
};
```