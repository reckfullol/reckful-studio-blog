---
title: HDU 1348 - Wall
date: utc
categories: HDU
---
# Wall

<!--more-->

## Problem Description

Once upon a time there was a greedy King who ordered his chief Architect to build a wall around the King's castle. The King was so greedy, that he would not listen to his Architect's proposals to build a beautiful brick wall with a perfect shape and nice tall towers. Instead, he ordered to build the wall around the whole castle using the least amount of stone and labor, but demanded that the wall should not come closer to the castle than a certain distance. If the King finds that the Architect has used more resources to build the wall than it was absolutely necessary to satisfy those requirements, then the Architect will loose his head. Moreover, he demanded Architect to introduce at once a plan of the wall listing the exact amount of resources that are needed to build the wall.
Your task is to help poor Architect to save his head, by writing a program that will find the minimum possible length of the wall that he could build around the castle to satisfy King's requirements.

![Wall](http://acm.hdu.edu.cn/data/images/1348-1.gif)

The task is somewhat simplified by the fact, that the King's castle has a polygonal shape and is situated on a flat ground. The Architect has already established a Cartesian coordinate system and has precisely measured the coordinates of all castle's vertices in feet.

## Input

The first line of the input file contains two integer numbers N and L separated by a space. N (3 <= N <= 1000) is the number of vertices in the King's castle, and L (1 <= L <= 1000) is the minimal number of feet that King allows for the wall to come close to the castle.

Next N lines describe coordinates of castle's vertices in a clockwise order. Each line contains two integer numbers Xi and Yi separated by a space (-10000 <= Xi, Yi <= 10000) that represent the coordinates of ith vertex. All vertices are different and the sides of the castle do not intersect anywhere except for vertices.

## Output

Write to the output file the single number that represents the minimal possible length of the wall in feet that could be built around the castle to satisfy King's requirements. You must present the integer number of feet to the King, because the floating numbers are not invented yet. However, you must round the result in such a way, that it is accurate to 8 inches (1 foot is equal to 12 inches), since the King will not tolerate larger error in the estimates.

This problem contains multiple test cases!

The first line of a multiple input is an integer N, then a blank line followed by N input blocks. Each input block is in the format indicated in the problem description. There is a blank line between input blocks.

The output format consists of N output blocks. There is a blank line between output blocks.

## Sample Input

```
1

9 100
200 400
300 400
300 300
400 300
400 400
500 400
500 200
350 200
200 200
```

## Sample Output

```
1628
```

## Solution

```cpp
#include <bits/stdc++.h>

double distance(std::pair<int, int> a, std::pair<int, int> b) {
    return sqrt(pow((a.first - b.first), 2.0) + pow((a.second - b.second), 2.0));
}

int multiply(std::pair<int, int> p2, std::pair<int, int> p1, std::pair<int, int> p0) {
    return (p2.first - p0.first) * (p1.second - p0.second) - (p1.first - p0.first) * (p2.second - p0.second);
}

int main() {
    std::ios::sync_with_stdio(false);

    const auto max_n = static_cast<unsigned int>(1e3+5);
    std::vector<std::pair<int, int>> point = std::vector<std::pair<int, int>>(max_n);
    std::vector<std::pair<int, int>> convex = std::vector<std::pair<int, int>>(max_n);
    int T;
    std::cin >> T;
    while(T--) {
        int n, l;
        std::cin >> n >> l;
        std::fill(point.begin(), point.begin() + n, std::pair<int, int>(0, 0));
        std::fill(convex.begin(), convex.begin() + n, std::pair<int, int>(0, 0));

        for(int i = 0; i < n; i++) {
            std::cin >> point[i].first >> point[i].second;
        }

        int index = 0;
        for(int i = 1; i < n; i++) {
            if(point[index].second > point[i].second || (point[index].second == point[i].second && point[index].first > point[i].first)) {
                index = i;
            }
        }

        std::swap(point[index], point[0]);
        std::sort(point.begin() + 1, point.begin() + n, [&point](std::pair<int, int> a, std::pair<int, int> b) {
            int ret = multiply(a, b, point[0]);
            return ret > 0 || (ret == 0 && distance(point[0], a) < distance(point[0], b));
        });

        for(int i = 0; i < 2; i++) {
            convex[i] = point[i];
        }

        index = 1;
        for(int i = 2; i < n; i++) {
            while(index >= 1 && multiply(point[i], convex[index], convex[index - 1]) >= 0) {
                index--;
            }
            convex[++index] = point[i];
        }

        double res = 2.0 * std::acos(-1.0) * l;
        for(int i = 1; i <= index; i++) {
            res += distance(convex[i], convex[i - 1]);
        }
        res += distance(convex[index], convex[0]);

        std::cout.setf(std::ios::fixed);
        std::cout.precision(0);
        std::cout << res << (T ? "\n\n" : "\n");
    }

    return 0;
}
```