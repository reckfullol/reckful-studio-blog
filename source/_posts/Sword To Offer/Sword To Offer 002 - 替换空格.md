---
title: Sword To Offer 002 - 替换空格
date: 2018-03-11 23:08:29
categories: Sword To Offer
---
# 替换空格

<!--more-->

## Desicription

请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

## Solution

```cpp
class Solution {
public:
    void replaceSpace(char *str,int length) {
        int blankCount = 0;
        int fontIndex = 0;
        for(; str[fontIndex]; fontIndex++) {
            if(str[fontIndex] == ' ') {
                blankCount++;
            }
        }
        realloc(str, static_cast<size_t>(fontIndex + (blankCount << 1) + 1));
        int tailIndex = fontIndex + (blankCount << 1);
        while(fontIndex >= 0) {
            if(str[fontIndex] != ' ') {
                str[tailIndex--] = str[fontIndex];
            }
            else {
                str[tailIndex--] = '0';
                str[tailIndex--] = '2';
                str[tailIndex--] = '%';
            }
            fontIndex--;
        }
    }
};

```