---
title: LeetCode 0142 - Linked List Cycle II
date: 2018-05-29 09:39:59
categories: LeetCode
---
# Linked List Cycle II

<!--more-->

## Desicription

Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

**Note**: Do not modify the linked list.

**Follow up**:

Can you solve it without using extra space?

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
    ListNode* detectCycle(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        ListNode* entry = head;
        while(fast && fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast) {
                while(slow != entry) {
                    slow = slow->next;
                    entry = entry->next;
                }
                return entry;
            }
        }
        return NULL;
    }
};
```