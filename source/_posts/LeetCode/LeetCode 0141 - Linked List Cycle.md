---
title: LeetCode 0141 - Linked List Cycle
date: 2018-05-29 09:38:18
categories: LeetCode
---
# Linked List Cycle

<!--more-->

## Desicription

Given a linked list, determine if it has a cycle in it.

Follow up:

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
    bool hasCycle(ListNode *head) {
        while(head) {
            if(head->val == INT_MIN)
                return true;
            head->val = INT_MIN;
            head = head->next;
        }
        return false;
    }
};
```