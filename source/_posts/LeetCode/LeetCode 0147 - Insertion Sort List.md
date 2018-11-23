---
title: LeetCode 0147 - Insertion Sort List
date: 2018-05-29 15:19:13
categories: LeetCode
---
# Insertion Sort List

<!--more-->

## Desicription


Sort a linked list using insertion sort.

![fuck](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)

A graphical example of insertion sort. The partial sorted list (black) initially contains only the first element in the list.
With each iteration one element (red) is removed from the input data and inserted in-place into the sorted list
 

**Algorithm of Insertion Sort**:

1. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
1. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
1. It repeats until no input elements remain.

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
    ListNode* insertionSortList(ListNode* head) {
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