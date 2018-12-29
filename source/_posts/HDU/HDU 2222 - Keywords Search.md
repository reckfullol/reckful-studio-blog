---
title: HDU 2222 - Keywords Search
date: 2018-12-29 21:01:56
categories: HDU
---
# Keywords Search

<!--more-->

## Desicription

In the modern time, Search engine came into the life of everybody like Google, Baidu, etc.
Wiskey also wants to bring this feature to his image retrieval system.
Every image have a long description, when users type some keywords to find the image, the system will match the keywords with description of image and show the image which the most keywords be matched.
To simplify the problem, giving you a description of image, and some keywords, you should tell me how many keywords will be match.

## Input

First line will contain one integer means how many cases will follow by.
Each case will contain two integers N means the number of keywords and N keywords follow. (N <= 10000)
Each keyword will only contains characters 'a'-'z', and the length will be not longer than 50.
The last line is the description, and the length will be not longer than 1000000.

## Output

Print how many keywords are contained in the description.

## Sample Input

```
1
5
she
he
say
shr
her
yasherhs
```

## Sample Output

```
3
```

## Solution

```cpp
#include <bits/stdc++.h>

struct Trie {

    static const int max_size = static_cast<const int>(5e5 + 5);
    int next[max_size][26];
    int fail[max_size];
    int end[max_size];
    int root;
    int index;

    int new_node() {
        for(int i = 0; i < 26; i++) {
            next[index][i] = -1;
        }
        end[index++] = 0;
        return index - 1;
    }

    void init() {
        index = 0;
        root = new_node();
    }

    void insert(const char buf[]) {
        int len = strlen(buf);
        int now = root;
        for(int i = 0; i < len; i++) {
            if(next[now][buf[i] - 'a'] == -1) {
                next[now][buf[i] - 'a'] = new_node();
            }
            now = next[now][buf[i] - 'a'];
        }
        end[now]++;
    }

    void build() {
        std::queue<int> Q;
        fail[root] = root;
        for(int i = 0; i < 26; i++) {
            if(next[root][i] == -1) {
                next[root][i] = root;
            } else {
                fail[next[root][i]] = root;
                Q.push(next[root][i]);
            }
        }
        while(!Q.empty()) {
            int now = Q.front();
            Q.pop();
            for(int i = 0; i < 26; i++) {
                if(next[now][i] == -1) {
                    next[now][i] = next[fail[now]][i];
                } else {
                    fail[next[now][i]] = next[fail[now]][i];
                    Q.push(next[now][i]);
                }
            }
        }
    }

    int query(const char buf[]) {
        int len = strlen(buf);
        int now = root;
        int res = 0;
        for(int i = 0; i < len; i++) {
            now = next[now][buf[i] - 'a'];
            int tmp = now;
            while(tmp != root) {
                res += end[tmp];
                end[tmp] = 0;
                tmp = fail[tmp];
            }
        }
        return res;
    }
};

Trie ac;

int main() {
    std::ios::sync_with_stdio(false);
    int T;
    std::cin >> T;
    while(T--) {
        int n;
        std::cin >> n;
        ac.init();
        std::string s;
        while(n--) {
            std::cin >> s;
            ac.insert(s.c_str());
        }
        ac.build();
        std::cin >> s;
        std::cout << ac.query(s.c_str()) << std::endl;
    }

    return 0;
}
```