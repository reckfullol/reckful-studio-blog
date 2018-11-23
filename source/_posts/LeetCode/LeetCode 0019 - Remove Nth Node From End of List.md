---
title: LeetCode 0019 - Remove Nth Node From End of List
date: 2017-11-09 20:13:02
categories: LeetCode
---
# Remove Nth Node From End of List #

<!--more-->

## Desicription ##

Given a linked list, remove the *nth* node from the end of list and return its head.

For example,

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given *n* will always be valid.
Try to do this in one pass.

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
class Solution
{
public:
    ListNode* removeNthFromEnd(ListNode* head, int n)
    {
        int total = 0;
        ListNode* traver = head;
        while(traver){
            total++;
            traver = traver->next;
        }
        n = total - n + 1;
        if(n == 1)
            return head->next;
        traver = head;
        int cnt = 1;
        while(traver){
            if(cnt == n - 1){
                traver->next = traver->next->next;
                return head;
            }
            traver = traver->next;
            cnt++;
        }
    }
};
```