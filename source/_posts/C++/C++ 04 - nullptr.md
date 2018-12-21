---
title: C++ 04 - nullptr
date: 2018-12-20 15:30:01
categories: C++
---
# nullptr

<!--more-->

整数用`0`, 实数用`0.0`, 指针用nullptr, 字符串用`'\0'`.

在标识地址(指针)的时候, C++11往后的项目用`nullptr`, C++0x项目则用`NULL`, 毕竟这看起来更像是指针.

因为在C++的基本观点上来看, `0`的型别是int, 而非指针. 至于说`NULL`, 不同的编译器实现各不相同, 并非都是int的整形型别(如long). 事实上不管实现的细节如何, 关键在于`0`和`NULL`都不具备指针型别.

举个例子:

```cpp
// 1
void f(int);

// 2
void f(bool);

// 3
void f(void*);

f(0);

f(NULL);

```

如果我们向这样的重载函数传递的时候, 就永远不会调到第三种函数.

f(NULL)的不确定性是NULL的型别在实现中的余地的一种反应. 假设NULL的定义为0L, 那么这个调用就有了多义性. 因此从long到int, 从long到bool, 还有从0L到void*的型别转换都是允许的.

`nullptr`的优点在于, 它不具备整形型别. 事实上nullptr的型别是std::nullptr_t, 他可以隐式转换到所有的裸指针型别.