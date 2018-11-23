---
title: LeetCode 0024 - Swap Nodes in Pairs
date: 2017-11-10 19:00:40
categories: LeetCode
---
# Swap Nodes in Pairs #

<!--more-->

## Desicription ##

Given a linked list, swap every two adjacent nodes and return its head.

For example,

Given `1->2->3->4`, you should return the list as `2->1->4->3`.

Your algorithm should use only constant space. You may **not** modify the values in the list, only nodes itself can be changed.

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
    ListNode* swapPairs(ListNode* head) {
        ListNode* travel = head;
        while(travel && travel->next){
            swap(travel->val, travel->next->val);
            travel = travel->next->next;
        }
        return head;
    }
};
```