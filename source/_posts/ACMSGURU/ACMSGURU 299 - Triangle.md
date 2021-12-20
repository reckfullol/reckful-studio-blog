---
title: ACMSGURU 299 - Triangle
date: 2019-12-18 10:44:15
categories: ACMSGURU
---
# Triangle

<!--more-->

## Problem Description

It is well known that three segments can make a triangle if and only if the sum of lengths of any two of them is strictly greater than the length of the third one. Professor Vasechkin has N segments. He asked you, if you could find at least one set of three segments among them which can be used by professor to make a triangle.

## Input

The first line of the input contains the only integer number N (3≤ N≤ 1000). The following N lines contain the length of segments professor has. The length of any segment is the integer number from 1 to 10^500.

## Output

Write to the output the length of segments requested by the professor — three numbers delimited by spaces. Write three zeros if there are no such three segments.

## Sample Input

7
1
2
6
4
8
100
73

## Sample Output

8 4 6

## Solution

```python
if __name__ == '__main__':
    n = int(input())
    array = []
    for i in range(n):
        array.append(int(input()))
    array.sort()

    for i in range(len(array)):
        for j in range(i + 1, len(array)):
            for k in range(j + 1, len(array)):
                if array[i] + array[j] > array[k]:
                    print(array[i], array[j], array[k])
                    exit(0)
                else:
                    break

    print(0, 0, 0)
    exit(0)
```

