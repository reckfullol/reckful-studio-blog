---
title: Sword To Offer 044 - 翻转单词顺序列
date: 2018-04-17 18:43:40
categories: Sword To Offer
---
# 翻转单词顺序列

<!--more-->

## Desicription

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

## Solution

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        string res;
        stack<char> tempStack;
        for(int i = str.size() - 1; i >= 0; i--) {
            if(str[i] == ' ') {
                while(!tempStack.empty()) {
                    res.push_back(tempStack.top());
                    tempStack.pop();
                }
                res.push_back(' ');
            } else {
                tempStack.push(str[i]);
            }
        }
        while(!tempStack.empty()) {
            res.push_back(tempStack.top());
            tempStack.pop();
        }
        return res;
    }
};
```