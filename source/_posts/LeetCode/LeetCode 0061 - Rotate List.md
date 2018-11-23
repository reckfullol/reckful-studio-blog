---
title: LeetCode 0061 - Rotate List
date: 2017-11-17 15:02:50
categories: LeetCode
---
# Rotate List #

<!--more-->

## Desicription ##

Given a list, rotate the list to the right by *k* places, where *k* is non-negative.


**Example:**

```
Given 1->2->3->4->5->NULL and k = 2,

return 4->5->1->2->3->NULL.
```

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
    ListNode* rotateRight(ListNode* head, int k) {
        int len = 0;
        ListNode *tail = NULL;
        for(ListNode* it = head; it; it = it->next)
            len++, tail = it;
        if(!len)
            return NULL;
        k = k % len;
        int cnt = 0;
        for(ListNode* it = head; it; it = it->next){
            cnt++;
            if(cnt == len - k){
                tail->next = head;
                head = it->next;
                it->next = NULL;
            }
        }
        return head;
    }
};
```