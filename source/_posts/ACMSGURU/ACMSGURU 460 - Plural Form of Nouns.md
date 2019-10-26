---
title: ACMSGURU 460 - Plural Form of Nouns
date: 2019-10-26 16:43:29
categories: ACMSGURU
---
# Plural Form of Nouns

<!--more-->

## Problem Description

In the English language, nouns are inflected by grammatical number — that is singular or plural. In this problem we use a simple model of constructing plural from a singular form. This model doesn't always make English plural forms correctly, but it works in most cases. Forget about the real rules you know while solving the problem and use the statement as a formal document.

You are given several nouns in a singular form and your program should translate them into plural form using the following rules:

- If a singular noun ends with ch, x, s, o the plural is formed by adding es. For example, witch -> witches, tomato -> tomatoes.

- If a singular noun ends with f or fe, the plural form ends with ves. For example, leaf -> leaves, knife -> knives. Pay attention to the letter f becoming v.

- Nouns ending with y change the ending to ies in plural. For example, family -> families.

- In all other cases plural is formed by adding s. For example, book -> books.


## Input

The first line of input contains a single positive integer n (1 ? n ? 10) — the number of words to be processed. The following n lines contain one word each. A word consists from 2 to 25 lowercase Latin letters. It is not guaranteed that the given words are real English words from vocabulary.

## Output

Print n given words in their plural forms on separate lines. Keep the words in the same order as they are given in the input.

## Example(s)

|sample input|sample output|
|--|--|
|3<br>contest<br>hero<br>lady|contests<br>heroes<br>ladies

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    int n{};
    std::cin >> n;
    while(n--) {
        std::string noun{};
        std::cin >> noun;

        if(!noun.empty()) {
            auto tmp = noun.substr(noun.size() - 1);
            if(tmp == "x" || tmp == "o" || tmp == "s") {
                std::cout << noun + "es" << std::endl;
                continue;
            } else if(tmp == "f") {
                std::cout << noun.substr(0, noun.size() - 1) + "ves" << std::endl;
                continue;
            } else if(tmp == "y") {
                std::cout << noun.substr(0, noun.size() - 1) + "ies" << std::endl;
                continue;
            }
        }

        if(noun.size() >= 2) {
            auto tmp = noun.substr(noun.size() - 2);
            if(tmp == "ch") {
                std::cout << noun + "es" << std::endl;
                continue;
            } else if(tmp == "fe") {
                std::cout << noun.substr(0, noun.size() - 2) + "ves" << std::endl;
                continue;
            }
        }

        std::cout << noun + "s" << std::endl;
    }
    return 0;
}
```