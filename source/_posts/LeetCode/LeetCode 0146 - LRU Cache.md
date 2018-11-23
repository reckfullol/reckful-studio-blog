---
title: LeetCode 0146 - LRU Cache
date: 2018-05-29 15:00:45
categories: LeetCode
---
# LRU Cache

<!--more-->

## Desicription

Design and implement a data structure for [Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU). It should support the following operations: `get` and `put`.

`get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.

`put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

**Follow up**:

Could you do both operations in **O(1)** time complexity?

**Example**:

```
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

## Solution

```cpp
class LRUCache {
private:
    int _capacity;
    list<int> used;
    unordered_map<int, pair<int, list<int>::iterator>> cache;

    void touch(unordered_map<int, pair<int, list<int>::iterator>>::iterator it) {
        used.erase(it->second.second);
        used.push_front(it->first);
        it->second.second = used.begin();
    }
public:
    LRUCache(int capacity) : _capacity(capacity) {};
    
    int get(int key) {
        auto it = cache.find(key);
        if(it == cache.end())
            return -1;
        touch(it);
        return it->second.first;
    }

    void put(int key, int value) {
        auto it = cache.find(key);
        if(it != cache.end())
            touch(it);
        else {
            if(cache.size() == _capacity) {
                cache.erase(used.back());
                used.pop_back();
            }
            used.push_front(key);
        }
        cache[key] = {value, used.begin()};
    }
};
```