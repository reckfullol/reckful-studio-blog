---
title: LeetCode 0148 - Sort List
date: 2018-05-29 15:21:22
categories: LeetCode
---
# Sort List

<!--more-->

## Desicription

Sort a linked list in O(*n log n*) time using constant space complexity.

**Example** 1:

```
Input: 4->2->1->3
Output: 1->2->3->4
```

**Example** 2:

```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
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
    ListNode* sortList(ListNode* head) {
        vector<int> vec;
        for(auto it = head; it != NULL; it = it->next)
            vec.push_back(it->val);    
        sort(vec.begin(), vec.end());
        int index = 0;
        for(auto it = head; it != NULL; it = it->next) 
            it->val = vec[index++];
        return head;
    }
};
```