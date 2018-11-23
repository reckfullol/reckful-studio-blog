---
title: LeetCode 0155 - Min Stack
date: 2018-06-02 11:58:09
categories: LeetCode
---
# Min Stack

<!--more-->

## Desicription

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

**Example**:

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

## Solution

```cpp
/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
class MinStack {
private:
    stack<int> numStack;
    stack<int> minStack;
public:
    void push(int x) {
        if(minStack.empty() || x <= minStack.top())
            minStack.push(x);
        numStack.push(x);
    }
    
    void pop() {
        if(numStack.top() == minStack.top())
            minStack.pop();
        numStack.pop();
    }
    
    int top() {
        return numStack.top();
    }
    
    int getMin() {
        return minStack.top();
    }
};
```