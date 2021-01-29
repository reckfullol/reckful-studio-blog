---
title: C++ 12 - polymorphic class copy
date: 2021-01-29 11:04:38
categories: C++
---
# polymorphic class copy

<!--more-->

多态类指存在至少一个虚函数继承关系的类. 当使用多态类的基类发生值拷贝时, 其隐式生成的拷贝构造函数和赋值运算符会导致一个问题: 派生类中只有基类部分被拷贝.

Example:
```cpp
class B { // BAD: polymorphic base class doesn’t suppress copying
public:
    virtual char m() { return ‘B’; }
    // … nothing about copy operations, so uses default …
};

class D : public B {
public:
    char m() override { return ‘D’; }
    // …
};

void f(B& b)
{
    auto b2 = b; // oops, slices the object; b2.m() will return ‘B’
}

D d;
f(d);
```

解决方法:

1. 实现virtual copy方法

```cpp
class B {
public:
    virtual owner<B*> clone() = 0;
    virtual ~B() = default;

    B(const B&) = delete;
    B& operator=(const B&) = delete;
};

class D : public B {
public:
    owner<D*> clone() override;
    ~D() override;
};
```

2. delete基类的拷贝构造函数和赋值运算符, 在编译器报错:

```cpp
class B { // GOOD: polymorphic class suppresses copying
public:
    B(const B&) = delete;
    B& operator=(const B&) = delete;
    virtual char m() { return ‘B’; }
    // …
};

class D : public B {
public:
    char m() override { return ‘D’; }
    // …
};

void f(B& b)
{
    auto b2 = b; // ok, compiler will detect inadvertent copying, and protest
}

D d;
f(d);
```