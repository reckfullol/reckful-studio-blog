---
title: LeetCode 1116 -  Print Zero Even Odd
date: 2019-12-28 01:31:11
categories: LeetCode
---
# Print Zero Even Odd

<!--more-->

## Desicription

Suppose you are given the following code:

```
class ZeroEvenOdd {
  public ZeroEvenOdd(int n) { ... }      // constructor
  public void zero(printNumber) { ... }  // only output 0's
  public void even(printNumber) { ... }  // only output even numbers
  public void odd(printNumber) { ... }   // only output odd numbers
}
```

The same instance of ZeroEvenOdd will be passed to three different threads:

1. Thread A will call zero() which should only output 0's.
2. Thread B will call even() which should only ouput even numbers.
3. Thread C will call odd() which should only output odd numbers.

Each of the threads is given a printNumber method to output an integer. Modify the given program to output the series 010203040506... where the length of the series must be 2n.

 

**Example 1:**

```
Input: n = 2
Output: "0102"
Explanation: There are three threads being fired asynchronously. One of them calls zero(), the other calls even(), and the last one calls odd(). "0102" is the correct output.
```

**Example 2:**

```
Input: n = 5
Output: "0102030405"
```

## Solution

```cpp
class ZeroEvenOdd {
private:
    int n_;
    int index_ = 1;
    std::mutex mutex_{};
    std::condition_variable condition_variable_{};

    bool is_zero_ = true;

public:
    ZeroEvenOdd(int n) {
        this->n_ = n;
    }

    // printNumber(x) outputs "x", where x is an integer.
    void zero(const std::function<void(int)>& printNumber) {
        while(index_ <= n_) {
            std::unique_lock unique_lock_{mutex_};
            condition_variable_.wait(unique_lock_, [&](){return is_zero_;});
            if(index_ > n_) {
                is_zero_ = false;
                condition_variable_.notify_all();
                return ;
            }
            printNumber(0);
            is_zero_ = false;
            condition_variable_.notify_all();
        }
    }

    void even(const std::function<void(int)>& printNumber) {
        while(index_ <= n_) {
            std::unique_lock unique_lock_{mutex_};
            condition_variable_.wait(unique_lock_, [&](){return !is_zero_ && (index_ % 2 == 0);});
            if(index_ > n_) {
                condition_variable_.notify_all();
                return ;
            }
            printNumber(index_++);
            is_zero_ = true;
            condition_variable_.notify_all();
        }
    }

    void odd(const std::function<void(int)>& printNumber) {
        while(index_ <= n_) {
            std::unique_lock unique_lock_{mutex_};
            condition_variable_.wait(unique_lock_, [&](){return !is_zero_ && (index_ % 2 == 1);});
            if(index_ > n_) {
                condition_variable_.notify_all();
                return ;
            }
            printNumber(index_++);
            is_zero_ = true;
            condition_variable_.notify_all();
        }
    }
};
```
