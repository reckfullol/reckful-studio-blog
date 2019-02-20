---
title: HDU 3662 - 3D Convex Hull
date: 2019-02-20 16:52:24
categories: HDU
---
# 3D Convex Hull

<!--more-->

## Desicription

There are N points in 3D-space which make up a 3D-Convex hull*. How many faces does the 3D-convexhull have? It is guaranteed that all the points are not in the same plane.

![convex](http://acm.hdu.edu.cn/data/images/C311-1003-1.jpg)

In case you don’t know the definition of convex hull, here we give you a clarification from Wikipedia: 
*Convex hull: In mathematics, the convex hull, for a set of points X in a real vector space V, is the minimal convex set containing X.

## Input

There are several test cases. In each case the first line contains an integer N indicates the number of 3D-points (3< N <= 300), and then N lines follow, each line contains three numbers x, y, z (between -10000 and 10000) indicate the 3d-position of a point.

## Output

Output the number of faces of the 3D-Convex hull.

## Sample Input

```
7
1 1 0
1 -1 0
-1 1 0
-1 -1 0
0 0 1
0 0 0
0 0 -0.1
7
1 1 0
1 -1 0
-1 1 0
-1 -1 0
0 0 1
0 0 0
0 0 0.1
```

## Sample Output

```
8
5
```

## Solution

```cpp
#include "bits/stdc++.h"

class Point {
public:
    double x, y, z;

public:
    Point() = default;

    Point(double out_x, double out_y, double out_z) : x(out_x), y(out_y), z(out_z) {};

    Point operator -(const Point& p) const {
        return Point(this->x - p.x, this->y - p.y, this->z - p.z);
    }

    Point operator +(const Point& p) const {
        return Point(this->x + p.x, this->y + p.y, this->z + p.z);
    }

    Point operator *(const Point& p) const {
        return Point(this->y * p.z - this->z * p.y, this->z * p.x - this->x * p.z, this->x * p.y - this->y * p.x);
    }

    Point operator /(const double& d) const {
        return Point(this->x / d, this->y / d, this->z / d);
    }

    double operator ^(const Point& p) const {
        return (this->x * p.x + this->y * p.y + this->z * p.z);
    }
};

class ConvexHull3D {
private:
    struct Surface {
        int a, b, c;
        bool isBelongToConvexHull;

        Surface(int out_a, int out_b, int out_c, bool outIsBelongToConvexHull) : a(out_a), b(out_b), c(out_c), isBelongToConvexHull(outIsBelongToConvexHull) {};
    };
    std::vector<Point> points;
    std::map<std::pair<double, double>, int> edgeToSurface;
    std::vector<Surface> surfaces;

    const double EPS = 1e-8;
private:
    static double vectorLength(const Point &p) {
        return sqrt(pow(p.x, 2) + pow(p.y, 2) + pow(p.z, 2));
    }

    static double triangleArea(const Point& a, const Point& b, const Point& c) {
        return vectorLength((b - a) * (c - a) / 2);
    }

    static double tetrahedronVolume(const Point& a, const Point& b, const Point& c, const Point& d) {
        return (b - a) * (c - a) ^ (d - a) / 6;
    }

    bool isPointSurfaceSameDirection(const Point &point, const Surface &surface) {
        return tetrahedronVolume(point, points[surface.a], points[surface.b], points[surface.c]) > EPS;
    }


public:
    double Area() {
        double res = 0;
        for(const Surface& surface : surfaces) {
            res += triangleArea(points[surface.a], points[surface.b], points[surface.c]);
        }
        return res;
    }

    double Volume() {
        double res = 0;
        for(const Surface& surface : surfaces) {
            res += tetrahedronVolume(Point(0, 0, 0), points[surface.a], points[surface.b], points[surface.c]);
        }
        return res;
    }

    int TriangleNumber() {
        return static_cast<int>(surfaces.size());
    }

    int PolygonNumber() {
        auto isSame = [this](const Surface& a, const Surface& b) {
            return  fabs(tetrahedronVolume(points[a.a], points[a.b], points[a.c], points[b.a])) < EPS &&
                    fabs(tetrahedronVolume(points[a.a], points[a.b], points[a.c], points[b.b])) < EPS &&
                    fabs(tetrahedronVolume(points[a.a], points[a.b], points[a.c], points[b.c])) < EPS;
        };
        int res = 0;
        for(int i = 0; i < surfaces.size(); i++) {
            bool flag = true;
            for(int j = 0; j < i; j++) {
                if(isSame(surfaces[i], surfaces[j])) {
                    flag = false;
                    break;
                }
            }
            res += flag;
        }
        return res;
    }

private:
    void updateConvexHull(const int& pointIndex, const std::pair<int, int>& edge) {
        Surface surface = surfaces[edgeToSurface[edge]];
        if(surface.isBelongToConvexHull) {
            if(isPointSurfaceSameDirection(points[pointIndex], surface)) {
                dfs(pointIndex, edgeToSurface[edge]);
            } else {
                edgeToSurface[std::pair<int, int>(pointIndex, edge.second)] = static_cast<int>(surfaces.size());
                edgeToSurface[std::pair<int, int>(edge.first, pointIndex)] = static_cast<int>(surfaces.size());
                edgeToSurface[std::pair<int, int>(edge.second, edge.first)] = static_cast<int>(surfaces.size());

                surfaces.emplace_back(edge.second, edge.first, pointIndex, true);
            }
        }
    }

    void dfs(const int& pointIndex, const int& surfaceIndex) {
        surfaces[surfaceIndex].isBelongToConvexHull = false;
        updateConvexHull(pointIndex, std::pair<int, int>(surfaces[surfaceIndex].b, surfaces[surfaceIndex].a));
        updateConvexHull(pointIndex, std::pair<int, int>(surfaces[surfaceIndex].c, surfaces[surfaceIndex].b));
        updateConvexHull(pointIndex, std::pair<int, int>(surfaces[surfaceIndex].a, surfaces[surfaceIndex].c));
    }

    void build() {
        bool isSameDirection = true;
        for(int i = 1; i < points.size(); i++) {
            if(vectorLength(points[0] - points[i]) > EPS) {
                std::swap(points[1], points[i]);
                isSameDirection = false;
                break;
            }
        }
        if(isSameDirection) {
            return ;
        }

        isSameDirection = true;
        for(int i = 2; i < points.size(); i++) {
            if(vectorLength((points[i] - points[0]) * (points[i] - points[1])) > EPS) {
                std::swap(points[2], points[i]);
                isSameDirection = false;
                break;
            }
        }
        if(isSameDirection) {
            return ;
        }

        isSameDirection = true;
        for(int i = 3; i < points.size(); i++) {
            if(fabs((points[1] - points[0]) * (points[2] - points[0]) ^ (points[i] - points[0])) > EPS) {
                std::swap(points[3], points[i]);
                isSameDirection = false;
                break;
            }
        }
        if(isSameDirection) {
            return ;
        }

        for(int i = 0; i < 4; i++) {
            Surface surface((i + 1) & 3, (i + 2) & 3, (i + 3) & 3, true);
            if(isPointSurfaceSameDirection(points[i], surface)) {
                std::swap(surface.b, surface.c);
            }
            edgeToSurface[std::pair<int, int>(surface.a, surface.b)] = static_cast<int>(surfaces.size());
            edgeToSurface[std::pair<int, int>(surface.b, surface.c)] = static_cast<int>(surfaces.size());
            edgeToSurface[std::pair<int, int>(surface.c, surface.a)] = static_cast<int>(surfaces.size());
            surfaces.push_back(surface);
        }

        for(int i = 4; i < points.size(); i++) {
            for(int j = 0; j < surfaces.size(); j++) {
                if(surfaces[j].isBelongToConvexHull && isPointSurfaceSameDirection(points[i], surfaces[j])) {
                    dfs(i, j);
                    break;
                }
            }
        }
        int index = 0;
        for (const Surface &surface : surfaces) {
            if(surface.isBelongToConvexHull) {
                surfaces[index++] = surface;
            }
        }
        surfaces.erase(surfaces.begin() + index, surfaces.end());
    }

public:
    explicit ConvexHull3D(const std::vector<Point>& out_points) {
        points = out_points;
        build();
    }
};

int main() {
    std::ios::sync_with_stdio(false);

    int n;
    while(std::cin >> n) {
        std::vector<Point> points;
        for(int i = 0; i < n; i++) {
            Point point{};
            std::cin >> point.x >> point.y >> point.z;
            points.push_back(point);
        }
        auto convexHull3D = new ConvexHull3D(points);
        std::cout << convexHull3D->PolygonNumber() << std::endl;
        delete convexHull3D;
    }

    return 0;
}
```