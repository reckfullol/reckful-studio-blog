---
title: POJ 3311 - Hie with the Pie
date: 2018-08-18 23:19:41
categories: POJ
---
# Hie with the Pie

<!--more-->

## Description

The Pizazz Pizzeria prides itself in delivering pizzas to its customers as fast as possible. Unfortunately, due to cutbacks, they can afford to hire only one driver to do the deliveries. He will wait for 1 or more (up to 10) orders to be processed before he starts any deliveries. Needless to say, he would like to take the shortest route in delivering these goodies and returning to the pizzeria, even if it means passing the same location(s) or the pizzeria more than once on the way. He has commissioned you to write a program to help him.

## Input

Input will consist of multiple test cases. The first line will contain a single integer n indicating the number of orders to deliver, where 1 ≤ n ≤ 10. After this will be n + 1 lines each containing n + 1 integers indicating the times to travel between the pizzeria (numbered 0) and the n locations (numbers 1 to n). The jth value on the ith line indicates the time to go directly from location i to location j without visiting any other locations along the way. Note that there may be quicker ways to go from i to j via other locations, due to different speed limits, traffic lights, etc. Also, the time values may not be symmetric, i.e., the time to go directly from location i to j may not be the same as the time to go directly from location j to i. An input value of n = 0 will terminate input.

## Output

For each test case, you should output a single number indicating the minimum time to deliver all of the pizzas and return to the pizzeria.

## Sample Input

```
3
0 1 10 10
1 0 1 2
10 1 0 10
10 2 10 0
0
```

## Sample Output

```
8
```

## Solution

```cpp
#include <iostream>
#include <limits.h>
using namespace std;

int n;
int mp[11][11];
int dp[1<<11][11];

void floyd() {
    for(int k = 0; k <= n; k++) {
        for(int i = 0; i <= n; i++) {
            for(int j = 0; j <= n; j++) {
                mp[i][j] = min(mp[i][j], mp[i][k] + mp[k][j]);
            }
        }
    }
}

void fun() {
    for(int i = 1; i < (1 << n); i++) {
        for(int j = 1; j <= n; j++) {
            int tmp = 1 << (j - 1);
            if(tmp == i) {
                dp[i][j] = mp[0][j];
            } else if(tmp & i) {
                dp[i][j] = INT_MAX;
                for(int k = 1; k <= n; k++) {
                    if(k != j && (i & (1 << (k - 1)))) {
                        dp[i][j] = min(dp[i][j], dp[i ^ tmp][k] + mp[k][j]);
                    }
                }
            }
        }
    }
    int x = (1 << n) - 1;
    int res = INT_MAX;
    for(int i = 1; i <= n; i++) {
        res = min(res, dp[x][i] + mp[i][0]);
    }
    cout << res << endl;
    return ;
}

int main() {
    while(cin >> n && n) {
        for(int i = 0; i <= n; i++) {
            for(int j = 0; j <= n; j++) {
                cin >> mp[i][j];
            }
        }
        floyd();
        fun();
    }
}
```