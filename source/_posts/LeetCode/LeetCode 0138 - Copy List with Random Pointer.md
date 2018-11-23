---
title: LeetCode 0138 - Copy List with Random Pointer
date: 2018-05-29 08:56:31
categories: LeetCode
---
# Copy List with Random Pointer

<!--more-->

## Desicription

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

## Solution

```cpp
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
class Solution {
public:
    RandomListNode* copyRandomList(RandomListNode* head) {
        if(head == NULL)
            return NULL;
        for(RandomListNode* l1 = head; l1 != NULL; l1 = l1->next) {
            RandomListNode* l2 = new RandomListNode(l1->label);
            l2->next = l1->random;
            l1->random = l2;
        }
        for(RandomListNode* l1 = head; l1 != NULL; l1 = l1->next) {
            RandomListNode* l2 = l1->random;
            l2->random = l2->next?l2->next->random:NULL;
        }
        RandomListNode* newHead = head->random;
        for(RandomListNode* l1 = head; l1 != NULL; l1 = l1->next) {
            RandomListNode* l2 = l1->random;
            l1->random = l2->next;
            l2->next = l1->next?l1->next->random:NULL;
        }
        return newHead;
    }
};
```