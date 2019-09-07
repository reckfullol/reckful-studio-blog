---
title: C++ 08 - copy elision
date: 2019-09-07 17:09:11
categories: C++
---
# copy elision

<!--more-->

> when a temporary class object that has not been bound to a reference (12.2) would be copied/moved to a class object with the same cv-unqualified type, the copy/move operation can be omitted by constructing the temporary object directly into the target of the omitted copy/move

当用一个临时对象初始化另一个对象的时候, 如果他们两个的 [cv-unqualified type](https://stackoverflow.com/questions/15413037/what-does-cv-unqualified-mean-in-c/15413274#15413274) 相同, 并且临时对象没有和任何引用绑定, 那么此次 copy/move construction 是可以省略的:

```cpp
struct X {
    X() {
        std::cout << "default construction" << std::endl;
    }

    ~X() {
        std::cout << "destructor" << std::endl;
    }

    X(const X&) {
        std::cout << "copy constructor" << std::endl;
    }

    X& operator=(const X&) {
        std::cout << "assignment operator" << std::endl;
    }
};

int main() {
    X x = X();
}
```

直接编译的结果:

```sh
RECKZHANG-MB1:cpp_test reckful$ g++ main.cc -o main 
RECKZHANG-MB1:cpp_test reckful$ ./main 
default construction
destructor
```

copy/move construction 都被省略.

加上 **-fno-elide-constructors** 编译选项:

```sh
RECKZHANG-MB1:cpp_test reckful$ g++ main.cc -o main -fno-elide-constructors
RECKZHANG-MB1:cpp_test reckful$ ./main 
default construction
copy constructor
destructor
copy constructor
destructor
destructor
```

class object 的两次拷贝构造分别发生在:

1. 函数返回值保存在一个临时变量, 构造这个临时变量的时候调用.

2. main 函数中的 x 变量构造的时候调用.

## 注意

为了简化问题, 我们假设的 gcc 版本是 4.1.2 而且不支持 C++0x 及后续版本提出的 move semantic.

在后续版本, 即使添加编译选项 **-fno-elide-constructors** 也会省略掉不必要掉 copy/move constructor.