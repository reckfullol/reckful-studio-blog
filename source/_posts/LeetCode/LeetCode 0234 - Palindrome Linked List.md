---
title: LeetCode 0234 - Palindrome Linked List
date: 2018-09-02 22:33:50
categories: LeetCode
---
# Palindrome Linked List

<!--more-->

## Desicription

Given a singly linked list, determine if it is a palindrome.

**Example 1:**

```
Input: 1->2
Output: false
```

**Example 2:**

```
Input: 1->2->2->1
Output: true
```

**Follow up:**

Could you do it in O(n) time and O(1) space?

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
private:
    ListNode* tmp = nullptr;
    bool check(ListNode* p) {
        if(nullptr == p) {
            return true;
        }
        bool flag = check(p->next) && (p->val == tmp->val);
        tmp = tmp->next;
        return flag;
    }
public:
    bool isPalindrome(ListNode* head) {
        tmp = head;
        return check(head);
    }
};
```