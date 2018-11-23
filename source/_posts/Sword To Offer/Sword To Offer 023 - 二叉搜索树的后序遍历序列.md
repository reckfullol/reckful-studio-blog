---
title: Sword To Offer 023 - 二叉搜索树的后序遍历序列
date: 2018-03-23 11:22:55
categories: Sword To Offer
---
# 二叉搜索树的后序遍历序列

<!--more-->

## Desicription

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

## Solution

```cpp
class Solution {
private:
    bool Verify(const vector<int>& sequence, int left, int right) {
        if(sequence.empty()) {
            return false;
        }
        int index = right - 1;
        for(; index >= left; index--) {
            if(sequence[index] < sequence[right]) {
                break;
            }
        }
        if(index < left) {
            return true;
        }
        for(int i = index + 1; i < right; i++) {
            if(sequence[i] < sequence[right]) {
                return false;
            }
        }
        for(int i = left; i <= index; i++) {
            if(sequence[i] > sequence[right]) {
                return false;
            }
        }
        return Verify(sequence, left, index) && Verify(sequence, index + 1, right - 1);
    }
public:
    bool VerifySquenceOfBST(const vector<int> &sequence) {
        return Verify(sequence, 0, sequence.size() - 1);
    }
};
```