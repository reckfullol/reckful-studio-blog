---
title: LeetCode 0143 - Reorder List
date: 2018-05-29 14:14:26
categories: LeetCode
---
# Reorder List

<!--more-->

## Desicription

Given a singly linked list L: L0→L1→…→Ln-1→Ln,

reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You may **not** modify the values in the list's nodes, only nodes itself may be changed.

**Example** 1:

```
Given 1->2->3->4, reorder it to 1->4->2->3.
```

**Example** 2:

```
Given 1->2->3->4->5, reorder it to 1->5->2->4->3.
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
    void reorderList(ListNode* head) {
        if(head == NULL)
            return ;
        vector<int> vec;
        for(auto it = head; it != NULL; it = it->next)
            vec.push_back(it->val);
        int left = 0;
        int right = vec.size()-1;
        for(auto it = head; it != NULL; it = it->next) {
            it->val = vec[left++];
            it = it->next;
            if(it == NULL)
                break;
            it->val = vec[right--];
        }
    }
};
```