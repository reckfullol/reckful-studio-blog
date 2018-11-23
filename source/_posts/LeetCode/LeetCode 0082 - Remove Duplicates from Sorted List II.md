---
title: LeetCode 0082 - Remove Duplicates from Sorted List II
date: 2017-11-20 21:38:19
categories: LeetCode
---
# Remove Duplicates from Sorted List II #

<!--more-->

## Desicription ##

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only *distinct* numbers from the original list.

For example,
Given `1->2->3->3->4->4->5`, return `1->2->5`.

Given `1->1->1->2->3`, return `2->3`.

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
        ListNode* fake_head = new ListNode(0);
        fake_head->next = head;
        ListNode* pre = fake_head;
        ListNode* now = head;
        while(now){
            while(now->next && now->val == now->next->val)
                now = now->next;
            if(pre->next == now)
                pre = pre->next;
            else
                pre->next = now->next;
            now = now->next;
        }
        return fake_head->next;
    }
};
```