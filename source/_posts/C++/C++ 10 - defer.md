---
title: C++ 10 - defer
date: 2020-12-25 20:54:47
categories: C++
---
# defer

<!--more-->

```cpp
#include <bits/stdc++.h>

class DoSomeThingWhenExit
{
public:
    explicit DoSomeThingWhenExit(std::function<void()> call_back_func) : on_exit_callback(std::move(call_back_func)) {}

    ~DoSomeThingWhenExit() { on_exit_callback(); }

private:
    std::function<void()> on_exit_callback;
};

#define GUARD_LINENAME_CAT(name, line) name##line
#define GUARD_LINENAME(name, line) GUARD_LINENAME_CAT(name, line)
#define DO_EXIT(callback) DoSomeThingWhenExit GUARD_LINENAME(EXIT, __LINE__)(callback)

int main() {
    std::ios::sync_with_stdio(false);

    // Replacement:
    // DoSomeThingWhenExit EXIT23([&]()
    //                           {
    //                               std::cout << 1 << std::endl;
    //                           })
    DO_EXIT([&](){
        std::cout << 1 << std::endl;
    });
    DO_EXIT([&](){
        std::cout << 2 << std::endl;
    });
    // 2
    // 1

    return 0;
}
```

实现方式: 
1. 传递函数闭包到类实例, 退出作用域时析构实现. 
2. 通过行号声明不同变量, 实现单作用域多defer.