---
title: LeetCode 0203 - Remove Linked List Elements
date: 2018-06-12 12:01:24
categories: LeetCode
---
# Remove Linked List Elements

<!--more-->

## Desicription

Remove all elements from a linked list of integers that have value **val**.

**Example**:

```
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
```

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
    ListNode* removeElements(ListNode* head, int val) {
        while(head) {
            if(head->val == val) {
                ListNode* tmp = head;
                head = head->next;
                free(tmp);
            }
            else
                break;
        }
        ListNode* trav = head;
        while(trav) {
            while(trav->next && trav->next->val == val) {
                ListNode* tmp = trav->next;
                trav->next = trav->next->next;
                free(tmp);
            }
            trav = trav->next;
        }
        return head;
    }
};
```