---
title: ACMSGURU 114 - Telecasting station
date: 2021-08-26 19:28:36
categories: ACMSGURU
---
# Telecasting station

<!--more-->

## Problem Description

Every city in Berland is situated on Ox axis. The government of the country decided to build new telecasting station. After many experiments Berland scientists came to a conclusion that in any city citizens displeasure is equal to product of citizens amount in it by distance between city and TV-station. Find such point on Ox axis for station so that sum of displeasures of all cities is minimal.

## Input

Input begins from line with integer positive number N (0<N<15000) – amount of cities in Berland. Following N pairs (X, P) describes cities (0<X, P<50000), where X is a coordinate of city and P is an amount of citizens. All numbers separated by whitespace(s).

## Output

Write the best position for TV-station with accuracy 10^-5.

## Sample Input

```
4
1 3
2 1
5 2
6 2
```

## Sample Output

```
3.00000
```

## Solution

```cpp
#include <bits/stdc++.h>
 
int main () {
    std::ios::sync_with_stdio(false);
 
    int n;
    std::cin >> n;
 
    double p_left = 0;
    double p_right = 0;
    double s_left = 0;
    double s_right = 0;
 
    std::vector<std::pair<double, double>> cities(n, std::pair<double, double>());
    for(auto& city : cities) {
        std::cin >> city.first >> city.second;
    }
 
    std::sort(cities.begin(), cities.end(), [](std::pair<double, double> a, std::pair<double, double> b) {
        return a.first < b.first;
    });
 
    for(int i = 0; i < n; i++) {
        p_right += cities[i].second;
        s_right += cities[i].first * cities[i].second;
    }
 
    double sum = LONG_LONG_MAX;
    double index = cities[0].first;
 
    for(int i = 0; i < n - 1; i++) {
        p_right -= cities[i].second;
        p_left += cities[i].second;
        s_right -= cities[i].first * cities[i].second;
        s_left += cities[i].first * cities[i].second;
        double a = cities[i].first * (p_left - p_right) + (s_right - s_left);
        double b = cities[i + 1].first * (p_left - p_right) + (s_right - s_left);
        if(sum - a > 1e-8) {
            sum = a;
            index = cities[i].first;
        }
 
        if(sum - b > 1e-8) {
            sum = b;
            index = cities[i + 1].first;
        }
    }
 
    std::cout<<std::setiosflags(std::ios::fixed);
    std::cout<<std::setprecision(5) << index << std::endl;
 
    return 0;
}
```