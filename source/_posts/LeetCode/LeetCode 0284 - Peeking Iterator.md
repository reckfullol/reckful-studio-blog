---
title: LeetCode 0284 - Peeking Iterator
date: 2018-12-18 17:04:14
categories: LeetCode
---
# Peeking Iterator

<!--more-->

## Desicription

Given an Iterator class interface with methods: `next()` and `hasNext()`, design and implement a PeekingIterator that support the peek() operation -- it essentially `peek()` at the element that will be returned by the next call to next().

**Example:**

```
Assume that the iterator is initialized to the beginning of the list: [1,2,3].

Call next() gets you 1, the first element in the list.
Now you call peek() and it returns 2, the next element. Calling next() after that still return 2. 
You call next() the final time and it returns 3, the last element. 
Calling hasNext() after that should return false.
```

## Solution

```cpp
// Below is the interface for Iterator, which is already defined for you.
// **DO NOT** modify the interface for Iterator.
class Iterator {
    struct Data;
    Data* data;
public:
    Iterator(const vector<int>& nums);
    Iterator(const Iterator& iter);
    virtual ~Iterator();
    // Returns the next element in the iteration.
    int next();
    // Returns true if the iteration has more elements.
    bool hasNext() const;
};


class PeekingIterator : public Iterator {
private:
    int index;
    vector<int> _nums;
public:
    PeekingIterator(const vector<int>& nums) : Iterator(nums) {
        // Initialize any member here.
        // **DO NOT** save a copy of nums and manipulate it directly.
        // You should only use the Iterator interface methods.
        _nums = nums;
        index = 0;
    }

    // Returns the next element in the iteration without advancing the iterator.
    int peek() {
        if(index < _nums.size()) {
            return _nums[index];
        } else {
            return -1;
        }
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    int next() {
        return index < _nums.size() ? _nums[index++] : -1;
    }

    bool hasNext() const {
        return index < _nums.size();
    }
};
```