---
title: LeetCode 0025 - Reverse Nodes in k-Group
date: 2017-11-10 19:06:29
categories: LeetCode
---
# Reverse Nodes in k-Group #

<!--more-->

## Desicription ##

Given a linked list, reverse the nodes of a linked list *k* at a time and return its modified list.

*k* is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.

Only constant memory is allowed.

For example,

Given this linked list: 1->2->3->4->5

For *k* = 2, you should return: `2->1->4->3->5`

For *k* = 3, you should return: `3->2->1->4->5`

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
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* travel = head;
        ListNode* res = new ListNode(0);
        ListNode* res_travel = res;
        vector<int>tmp;
        while(travel){
            tmp.push_back(travel->val);
            if(tmp.size() == k){
                for(int i = tmp.size()-1; i >= 0; i--){
                    res_travel->next = new ListNode(tmp[i]);
                    res_travel = res_travel->next;
                }
                tmp.clear();
            }
            travel = travel->next;
        }
        if(!tmp.empty())
            for(int i = 0; i < tmp.size(); i++)
                res_travel->next = new ListNode(tmp[i]), res_travel = res_travel->next;
        return res->next;
    }
};
```