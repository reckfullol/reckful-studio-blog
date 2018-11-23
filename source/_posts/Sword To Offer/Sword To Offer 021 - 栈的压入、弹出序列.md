---
title: Sword To Offer 021 - 栈的压入、弹出序列
date: 2018-03-22 20:04:38
categories: Sword To Offer
---
# 栈的压入、弹出序列

<!--more-->

## Desicription

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4，5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

## Solution

```cpp
class Solution {
public:
    bool IsPopOrder(const vector<int>& pushV, const vector<int>& popV) {
        stack<int> _stack;
        int _pushIndex = 0;
        for (int i : popV) {
            while(_stack.empty() || _stack.top() != i) {
                _stack.push(pushV[_pushIndex++]);
                if(_pushIndex > pushV.size()) {
                    return false;
                }
            }
            _stack.pop();
        }
        return _stack.empty();
    }
};
```