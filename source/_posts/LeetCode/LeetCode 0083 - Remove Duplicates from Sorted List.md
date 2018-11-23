---
title: LeetCode 0083 - Remove Duplicates from Sorted List
date: 2017-11-20 21:41:01
categories: LeetCode
---
# Remove Duplicates from Sorted List #

<!--more-->

## Desicription ##

Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,

Given `1->1->2`, return `1->2`.

Given `1->1->2->3->3`, return `1->2->3`.

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
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* now = head;
        while(now){
            while(now->next && now->val == now->next->val){
                ListNode* tmp = now->next;
                now->next = tmp->next;
                free(tmp);
            }
            now = now->next;
        }
        return head;
    }
};
```