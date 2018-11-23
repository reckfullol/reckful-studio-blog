---
title: LeetCode 0023 - Merge k Sorted Lists
date: 2017-11-10 18:56:12
categories: LeetCode
---
# Merge k Sorted Lists #

<!--more-->

## Desicription ##

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

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
private:
struct cmp{
    bool operator()(ListNode const *a, ListNode const *b){
        return a->val > b->val;
    }
};

public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode *head = new ListNode(0);
        ListNode *now(head);
        priority_queue<ListNode*, vector<ListNode*>, cmp>Q;
        for(int i = 0; i < lists.size(); i++)
            if(lists[i])
                Q.push(lists[i]);
        while(!Q.empty()){
            ListNode *top = Q.top();
            Q.pop();
            now->next = new ListNode(0);
            now->next->val = top->val;
            now = now->next;
            if(top->next)
                Q.push(top->next);
        }
        return head->next;
    }
};
```