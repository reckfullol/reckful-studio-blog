---
title: LeetCode 0092 - Reverse Linked List II
date: 2017-12-15 11:36:54
categories: LeetCode
---
# Reverse Linked List II #

<!--more-->

## Desicription ##

Reverse a linked list from position *m* to *n*. Do it in-place and in one-pass.

For example:
Given `1->2->3->4->5->NULL`, *m* = 2 and *n* = 4,

return `1->4->3->2->5->NULL`.

**Note:**
Given *m*, *n* satisfy the following condition:
1 ≤ *m* ≤ *n* ≤ length of list.

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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        ListNode* res = new ListNode(0);
        res->next = head;
        ListNode* cur = res;
        int cnt = 0;
        while(cur) {
            if(cnt + 1 == m) {
                ListNode* tmp = cur->next;
                vector<int> vec;
                while(cnt != n){
                    vec.push_back(tmp->val);
                    tmp = tmp->next, cnt++;
                }
                for(int i = vec.size() - 1; i >= 0; i--)
                    cur->next = new ListNode(vec[i]), cur = cur->next;
                cur->next = tmp;
                return res->next;
            }
            cur = cur->next, cnt++;
        }
    }
};

```