---
title: ACMSGURU 397 - Text Editor
date: 2021-12-03 18:13:13
categories: ACMSGURU
---
# Preparing Problem

<!--more-->

## Problem Description

The simplest text editor "Open Word" allows to create and edit only one word. The editor processes keys 'a' -- 'z', and also 'L' (to the left) and 'R' (to the right). After starting his work the editor immediately creates an empty word and sets its cursor to the left-most position. When one of keys 'a' -- 'z' is pressed, the text editor inserts corresponding symbol just after the cursor. After that a cursor moves one position to the right in such a way that it is placed just after new symbol. When key 'L' or 'R' is pressed, the cursor moves one position to the left or to the right respectively. If the cursor can't be moved because it is placed at the left-most or right-most position the command is ignored. Developers of "Open Word" didn't think about the effectiveness so the editor is working slowly if a lot of keys have been pressed.

Your task is to write a program that can process a sequence of key pressings emulating this editor and output result string.

## Input

The input file contains one string which consists of symbols 'a' -- 'z', 'L' and 'R'. The string length is not less than 1 and doesn't exceed 10^6.

## Output

Write a required string to the output file.

## Example(s)

|sample input|sample output|
|--|--|
|abLcd|acdb|

|sample input|sample output|
|-|-|
|icpLLLLLacmRRRRRRRRRRRRc|acmicpc|

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    std::string input;
    std::cin >> input;

    std::list<char> text;
    auto it = text.begin();
    for(const auto& c : input) {
        if(c == 'L') {
            if(it != text.begin()) {
                it--;
            }
            continue;
        }
        if(c == 'R') {
            if(it != text.end()) {
                it++;
            }
            continue;
        }
        text.insert(it, c);
    }
    for(const auto& c : text) {
        std::cout << c;
    }
}
```