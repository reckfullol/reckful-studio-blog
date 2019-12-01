---
title: LeetCode 1115 - Print FooBar Alternately
date: 2019-11-30 00:55:37
categories: LeetCode
---
# Print FooBar Alternately

<!--more-->

## Desicription

Suppose you are given the following code:

```cpp
class FooBar {
  public void foo() {
    for (int i = 0; i < n; i++) {
      print("foo");
    }
  }

  public void bar() {
    for (int i = 0; i < n; i++) {
      print("bar");
    }
  }
}
```
The same instance of FooBar will be passed to two different threads. Thread A will call foo() while thread B will call bar(). Modify the given program to output "foobar" n times.

## Example 1:

```
Input: n = 1
Output: "foobar"
Explanation: There are two threads being fired asynchronously. One of them calls foo(), while the other calls bar(). "foobar" is being output 1 time.
```

## Example 2:

```
Input: n = 2
Output: "foobarfoobar"
Explanation: "foobar" is being output 2 times.
```

## Solution

```cpp
class FooBar {
private:
    int n;
    bool flag = false;
    std::mutex mutex{};
    std::condition_variable conditionVariable{};
public:
    FooBar(int n) {
        this->n = n;
    }

    void foo(std::function<void()> printFoo) {

        for (int i = 0; i < n; i++) {
            std::unique_lock uniqueLock{mutex};
            if(flag == true) {
                conditionVariable.wait(uniqueLock);
            }
            // printFoo() outputs "foo". Do not change or remove this line.
            printFoo();
            flag = true;
            conditionVariable.notify_all();
        }
    }

    void bar(std::function<void()> printBar) {

        for (int i = 0; i < n; i++) {
            std::unique_lock uniqueLock{mutex};
            if(flag == false) {
                conditionVariable.wait(uniqueLock);
            }
            // printBar() outputs "bar". Do not change or remove this line.
            printBar();
            flag = false;
            conditionVariable.notify_all();
        }
    }
};
```
