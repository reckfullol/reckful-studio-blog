---
title: C++ 06 - new
date: 2019-01-25 00:20:58
categories: C++
---
# new

<!--more-->

关于c++中的new, 主要分为`operator new` 和 `new operator`.

## new operator

new operator就是标准new:

    1. 调用new从堆中找到合适的内存空间进行分配, 同时调用构造函数.
    2. 不允许被重载.

## operator new

operator new是函数:

    1. 只分配内存空间, 不调用构造函数, 当没有满足的空间进行分配时, 调用new_handler(), 如果new_handler()没有定义, 执行bad_alloc异常.
    2. 可以被重载.
    3. 重载时返回类型需要是void*, 表示分配的地址. 第一个参数类型是size_t, 表示申请空间大小, 可以带其他参数.
    4. 值得注意的是, 虽然重载的operator new不会调用构造函数, 但是当operator new return的时候, 编译器会自动调用对象的构造函数.

## placement new

placement new是重载operator new的一个标准, 全局的版本, 不能被自定义版本代替.

```cpp
void* operator new(size_t, void* pointer) {
    return pointer;
}
```

placement new的执行忽略了size_t参数, 只返还第二个参数, 其结果是允许用户把申请的对象放在一个指定的内存空间, 用法如下:

```cpp
auto buffer_add = malloc(sizeof(Foo) + sizeof(int));
auto foo = new(buffer_add) Foo;
``` 
