---
title: Sword To Offer 020 - 包含min函数的栈
date: 2018-03-22 18:59:08
categories: Sword To Offer
---
# 包含min函数的栈

<!--more-->

## Desicription

定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。

## Solution

```cpp
class Solution {
private:
    stack<int> _stack;
    stack<int> _minStack;
public:
    void push(int value) {
        _stack.push(value);
        if(_minStack.empty()) {
            _minStack.push(value);
        } else {
            if(value < _minStack.top()) {
                _minStack.push(value);
            }
        }
    }
    void pop() {
        if(_stack.top() == _minStack.top()) {
            _stack.pop();
            _minStack.pop();
        } else {
            _stack.pop();
        }
    }
    int top() {
        return _stack.top();
    }
    int min() {
        return _minStack.top();
    }
};
```