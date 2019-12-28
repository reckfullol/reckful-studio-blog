---
title: LeetCode 1195 -  Fizz Buzz Multithreaded
date: 2019-12-28 21:21:09
categories: LeetCode
---
# Fizz Buzz Multithreaded

<!--more-->

## Desicription

Write a program that outputs the string representation of numbers from 1 to n, however:

- If the number is divisible by 3, output "fizz".
- If the number is divisible by 5, output "buzz".
- If the number is divisible by both 3 and 5, output "fizzbuzz".

For example, for n = 15, we output: 1, 2, fizz, 4, buzz, fizz, 7, 8, fizz, buzz, 11, fizz, 13, 14, fizzbuzz.

Suppose you are given the following code:

```
class FizzBuzz {
  public FizzBuzz(int n) { ... }               // constructor
  public void fizz(printFizz) { ... }          // only output "fizz"
  public void buzz(printBuzz) { ... }          // only output "buzz"
  public void fizzbuzz(printFizzBuzz) { ... }  // only output "fizzbuzz"
  public void number(printNumber) { ... }      // only output the numbers
}
```

Implement a multithreaded version of FizzBuzz with four threads. The same instance of FizzBuzz will be passed to four different threads:

1. Thread A will call fizz() to check for divisibility of 3 and outputs fizz.
2. Thread B will call buzz() to check for divisibility of 5 and outputs buzz.
3. Thread C will call fizzbuzz() to check for divisibility of 3 and 5 and outputs fizzbuzz.
4. Thread D will call number() which should only output the numbers.

## Solution

```cpp
class FizzBuzz {
private:
    int n;
    int index = 1;
    std::mutex mutex_{};
    std::condition_variable condition_variable_{};
public:
    FizzBuzz(int n) {
        this->n = n;
    }

    // printFizz() outputs "fizz".
    void fizz(const std::function<void()>& printFizz) {
        while(index <= n) {
            std::unique_lock unique_lock_{mutex_};
            condition_variable_.wait(unique_lock_, [&](){
                return (index % 3 == 0 && index % 5 != 0) || index > n;
            });
            if(index > n) {
                condition_variable_.notify_all();
                return ;
            }
            printFizz();
            index += 1;
            condition_variable_.notify_all();
        }
    }

    // printBuzz() outputs "buzz".
    void buzz(const std::function<void()>& printBuzz) {
        while(index <= n) {
            std::unique_lock unique_lock_{mutex_};
            condition_variable_.wait(unique_lock_, [&](){
                return (index % 3 != 0 && index % 5 == 0) || index > n;
            });
            if(index > n) {
                condition_variable_.notify_all();
                return ;
            }
            printBuzz();
            index += 1;
            condition_variable_.notify_all();
        }
    }

    // printFizzBuzz() outputs "fizzbuzz".
    void fizzbuzz(const std::function<void()>& printFizzBuzz) {
        while(index <= n) {
            std::unique_lock unique_lock_{mutex_};
            condition_variable_.wait(unique_lock_, [&](){
                return (index % 3 == 0 && index % 5 == 0) || index > n;
            });
            if(index > n) {
                condition_variable_.notify_all();
                return ;
            }
            printFizzBuzz();
            index += 1;
            condition_variable_.notify_all();
        }
    }

    // printNumber(x) outputs "x", where x is an integer.
    void number(const std::function<void(int)>& printNumber) {
        while(index <= n) {
            std::unique_lock unique_lock_{mutex_};
            condition_variable_.wait(unique_lock_, [&](){
                return index % 3 != 0 && index % 5 != 0;
            });
            if(index > n) {
                condition_variable_.notify_all();
                return ;
            }
            printNumber(index);
            index += 1;
            condition_variable_.notify_all();
        }
    }
};
```
