---
title: C++ 11 - singleton
date: 2020-12-31 16:32:14
categories: C++
---
# singleton

<!--more-->

```cpp
// T must be: no-throw default constructible and no-throw destructible
template <typename T>
struct Singleton
{
private:
    struct object_creator
    {
        // This constructor does nothing more than ensure that instance()
        //  is called before main() begins, thus creating the static
        //  T object before multithreading race issues can come up.
        object_creator() { Singleton<T>::GetInst(); }
        inline void do_nothing() const {}
    };
    static object_creator create_object;

protected:
    ~Singleton() = default;
    Singleton() = default;

public:
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

public:
    // If, at any point (in user code), Singleton<T>::instance()
    //  is called, then the following function is instantiated.
    static T& GetInst()
    {
        // This is the object that we return a reference to.
        // It is guaranteed to be created before main() begins because of
        //  the next line.
        static T obj;

        // The following line does nothing else than force the instantiation
        //  of Singleton<T>::create_object, whose constructor is
        //  called before main() begins.
        create_object.do_nothing();

        return obj;
    }
};
template <typename T>
typename Singleton<T>::object_creator Singleton<T>::create_object;
```

- 删除拷贝构造函数和赋值运算符, 隐藏构造函数, 约束实例唯一.
- 单例模板添加`do_nothing`函数调用, 保证单例实例构造调用在main调用之前.