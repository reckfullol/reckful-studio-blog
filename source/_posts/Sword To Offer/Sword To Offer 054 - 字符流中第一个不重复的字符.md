---
title: Sword To Offer 054 - 字符流中第一个不重复的字符
date: 2018-05-12 17:04:52
categories: Sword To Offer
---
# 字符流中第一个不重复的字符

<!--more-->

## Desicription

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

输出描述:

如果当前字符流没有存在出现一次的字符，返回#字符。

## Solution

```cpp
class Solution {
private:
    unordered_map<int, int> book;
    queue<int> order;
public:
    //Insert one char from stringstream
    void Insert(char ch) {
        book[ch-'a']++;
        if(book[ch - 'a'] == 1) {
            order.push(ch - 'a');
        }
    }
    //return the first appearence once char in current stringstream
    char FirstAppearingOnce() {
        while(!order.empty()) {
            if(book[order.front()] > 1) {
                order.pop();
            } else {
                return static_cast<char>(order.front() + 'a');
            }
        }
        return '#';
    }
};
```