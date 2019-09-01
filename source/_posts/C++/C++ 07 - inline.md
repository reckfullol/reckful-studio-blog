---
title: C++ 07 - inline
date: 2019-09-01 12:52:31
categories: C++
---
# inline

<!--more-->

inline 函数是一种编程语言结构, 用来建议编译器对一些特殊函数进行内联扩展, 即将指定对函数体插入并取代每一处调用该函数的地方(上下文), 从而节省每次调用函数带来的额外时间开支.

## C++语法

明确声明 inline函数的做法是在其定义式前加上关键字inline, 例如:

```cpp
template<typename T>
inline const T& std::max(const T& am const T& b) {
    return a < b ? b : a;
}
```

inline 函数通常一定被置于头文件内, 因为大多数 build environment 在编译过程中进行 inlining, 为了将一个函数调用替换为被调用函数的本体, 编译器必须知道那个函数长什么样子. 某些 build environment 可以在连接期完成 inlining, 少量 build environment 如基于 .NET CLI 的 managed environments 可以在运行期完成 inlining, 但是大部分的 C++程序是在编译期完成 inlining.

inline 只是对编译器的一个申请, 并不是强制命令. 这项申请可以隐喻的提出, 隐喻方式是将函数定义于 class 定义式内:

```cpp
class Person {
public:
    /**
     * 一个隐喻的 inline 申请:
     * 成员函数在类内被定义
     */
    int GetAge() {
        return age;
    }

private:
    int age;
}
```

大部分拒绝将太过复杂的函数 inlining, 例如带有循环或者递归的函数, 而对于所有的 virtual 函数也会失效, 因为 virtual 意味着直到运行期才能确定调用哪个函数.

有时候编译器虽然有意愿 inlining 某个函数, 还是可能为该函数生成一个函数实体. 比如如果程序要取某个 inline 函数的地址, 编译器通常必须为此函数生成一个 outlined 函数本体. 毕竟编译器没有能力提出一个指针指向并不存在的函数. 于此并提的是, 编译器通常不对通过函数指针而进行的调用实施 inlining, 这意味这对 inline 函数的调用有可能被 inlined, 也可能不被 inlined , 取决于调用的方式:

```cpp
/**
 * 编译器有意愿 inline 对f的调用
 */
inline void f() {...}

/**
 * pf 指向 f
 */
void(*pf) () = f;

/**
 * 这个调用将被 inlined, 因为他是一个正常调用
 */
f();

/**
 * 这个调用或许不被 inlined, 因为他通过函数指针达成
 */
pf();
```

## 优点

1. inline 函数的代码被放入符号表中, 在使用时进行替换, 避免了函数调用时的开销.

2. 编译器在调用内联函数的时候, 会对其合法性进行检查, 保证调用的正确, 消除了隐患和局限性.

3. inline 可以作为类的成员函数, 因此可以使用所在类的 protect 成员和 private 成员.

## 缺点

inline 的使用会增加 object code 大小. 在内存有限的机器上, 过度热衷 inlining 会造成程序体积过大(对可用空间而言), 即使拥有虚拟内存, inline 造成的代码膨胀亦会导致额外的 paging, 降低 instruction cache hit rate, 以及伴随这些而来的效率问题.

## 注意

1. 构造函数和析构函数往往是 inlining 的糟糕候选人, 例如:

    ```cpp
    class Base {
    private:
        std::string bm1, bm2;
    };

    class Derived: public Base {
    public:
        Derived() {

        };
    private:
        std::string bm1, bm2, bm3;
    };
    ```

    这个 Derived() 的构造函数是 inline 的绝佳候选人, 因为他不包含任何代码, 但其实 C++ 对于对象被创建和被销毁时发生什么事做了各种各样的保证. 当使用 new, 动态创建的对象被其构造函数自动初始化, 当使用 delete, 对应的析构函数会被调用. 当你创建一个对象, 每一个base class 及其每一个成员变量都会被自动构造, 当你销毁一个对象, 反向程序的析构行为亦会自动发生. 如果有个异常在对象构造期间被跑出, 该对象已构造好的那一部分会被自动销毁. 所以我们可以想象, 编译器为了表面上看起来空的 Derived 构造函数所产生的代码:

    ```cpp
    class Base {
    private:
        std::string bm1, bm2;
    };

    class Derived: public Base {
    public:
        Derived() {
            Base::Base();
            
            try {
                dm1.std::string::string();
            } catch (...) {
                Base::~Base();
                throw;
            }
            
            try {
                dm2.std::string::string();
            } catch (...) {
                dm2.std::string::~string();
                Base::~Base();
                throw;
            }
            
            try {
                dm3.std::string::string();
            } catch (...) {
                dm1.std::string::~string();
                dm2.std::string::~string();
                dm3.std::string::~string();
                Base::~Base();
                throw;
            }
        };
    private:
        std::string bm1, bm2, bm3;
    };
    ```

    这段代码并不能代表编译器真正制造出来的代码, 只能描述观念性实现, 因为真正的编译器会以更精致复杂的做法来处理一场. 尽管如此, 这已能准确反应 Derived 的空白构造函数必须提供的行为. 因此如果他被 inlined, 隐形 object code 会急剧膨胀. 现在我们可以看到, 将构造函数和析构函数 inline 化并不是一个轻松的决定.

2. 程序设计者必须知道, inline 函数无法跟随程序库的升级而升级

    如果 f 是程序库内一个 inline 函数, 客户将 f 函数本体编进其程序中, 一旦程序库设计者决定改变 f, 所有用到 f 的客户端程序都必须重新编译. 而如果 f 是 non-inline 函数, 一旦它有任何修改, 客户端只需重新连接就好, 远比重新编译的负担少很多. 如果程序库采用动态连接, 升级版函数甚至可以不知不觉的被应用程序采纳.