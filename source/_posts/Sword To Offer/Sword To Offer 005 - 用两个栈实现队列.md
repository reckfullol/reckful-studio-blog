---
title: Sword To Offer 005 - 用两个栈实现队列
date: 2018-03-13 22:13:42
categories: Sword To Offer
---
# 用两个栈实现队列

<!--more-->

## Desicription

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

## Solution

```cpp
class Solution {
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        if(stack2.empty()) {
            while(!stack1.empty()) {
                stack2.push(stack1.top());
                stack1.pop();
            }
        }
        int res = stack2.top();
        stack2.pop();
        return res;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```