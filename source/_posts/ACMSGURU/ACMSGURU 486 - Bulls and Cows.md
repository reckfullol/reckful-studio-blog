---
title: ACMSGURU 486 - Bulls and Cows
date: 2019-10-28 20:36:49
categories: ACMSGURU
---
# Bulls and Cows

<!--more-->

## Problem Description

You probably know the game "bulls and cows". Just in case, we explain the rules. The first player picks a four-digit number with all digits distinct (leading zero is allowed) and keeps it secret. The second player tries to guess the secret number. For each guess, the first player issues a response in the form "n bulls, m cows". A "bull" is a digit that is present in both the secret and the guess and occurs in the same position in both. A "cow" is a digit that is present in both numbers, but occurs in different positions.

For example, if the first player picked 5071, and the second guessed 6012, the response would be "one bull, one cow". Here the "bull" is the digit 0, as it is in the second position in both numbers, and the "cow" is the digit 1, as it is in the fourth position in the secret, but in the third position in the guess.

Write a program to count the number of cows and bulls for the given the secret and guess.

## Input
The first line of the input file contains four digits, the number picked by the first player. The second line contains the number guessed by the second player in the same format.

## Output
The first and only line of the output file should contain two integers separated by a space, the number of "bulls" and the number of "cows".

## Example(s)

|sample input|sample output|
|--|--|
|5071<br>6012|1 1|

|sample input|sample output|
|--|--|
|4321<br>4321|4 0|

|sample input|sample output|
|--|--|
|1980<br>0879|0 3|

|sample input|sample output|
|--|--|
|1234<br>5678|0 0|

## Solution

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);

    std::string a{};
    std::string b{};

    std::cin >> a >> b;
    std::vector<int> countA = std::vector<int>(10, 0);
    std::vector<int> countB = std::vector<int>(10, 0);

    int bulls = 0;
    int cows = 0;
    for(int i = 0; i < 4; i++) {
        countA[a[i] - '0'] += 1;
        countB[b[i] - '0'] += 1;
        if(a[i] == b[i]) {
            bulls += 1;
        }
    }

    for(int i = 0; i < 10; i++) {
        if(countA[i] != 0 && countB[i] != 0) {
            cows += std::min(countA[i], countB[i]);
        }
    }

    std::cout << bulls << " " << cows - bulls << std::endl;
    return 0;
}
```