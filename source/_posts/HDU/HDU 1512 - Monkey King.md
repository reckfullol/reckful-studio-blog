---
title: HDU 1512 - Monkey King
date: 2019-02-26 18:23:30
categories: HDU
---
# Monkey King

<!--more-->

## Desicription

Once in a forest, there lived N aggressive monkeys. At the beginning, they each does things in its own way and none of them knows each other. But monkeys can't avoid quarrelling, and it only happens between two monkeys who does not know each other. And when it happens, both the two monkeys will invite the strongest friend of them, and duel. Of course, after the duel, the two monkeys and all of there friends knows each other, and the quarrel above will no longer happens between these monkeys even if they have ever conflicted.

Assume that every money has a strongness value, which will be reduced to only half of the original after a duel(that is, 10 will be reduced to 5 and 5 will be reduced to 2).

And we also assume that every monkey knows himself. That is, when he is the strongest one in all of his friends, he himself will go to duel.

## Input

There are several test cases, and each case consists of two parts.

First part: The first line contains an integer N(N<=100,000), which indicates the number of monkeys. And then N lines follows. There is one number on each line, indicating the strongness value of ith monkey(<=32768).

Second part: The first line contains an integer M(M<=100,000), which indicates there are M conflicts happened. And then M lines follows, each line of which contains two integers x and y, indicating that there is a conflict between the Xth monkey and Yth.

## Output

For each of the conflict, output -1 if the two monkeys know each other, otherwise output the strongness value of the strongest monkey in all friends of them after the duel.

## Sample Input

```
5
20
16
10
10
4
5
2 3
3 4
3 5
4 5
1 5
```

## Sample Output

```
8
5
5
-1
10
```

## Solution

```cpp
#ifndef ONLINE_JUDGE
#include "my_stdc++.h"
#else
#include <bits/stdc++.h>
#endif

const int MaxN = static_cast<const int>(1e5 + 5);

class LeftBiasTree;

extern LeftBiasTree Pool[];
int PoolTail = 0;

class LeftBiasTree {
private:
    LeftBiasTree* left;
    LeftBiasTree* right;
    int value;
    int distance;
public:
    explicit LeftBiasTree(int outValue = 0) : left(nullptr), right(nullptr), value(outValue), distance(0) {
    };

    static LeftBiasTree* Merge(LeftBiasTree* a, LeftBiasTree* b) {
        if(a == nullptr) {
            return b;
        }

        if(b == nullptr) {
            return a;
        }

        if(a->value < b->value) {
            std::swap(a, b);
        }

        a->right = Merge(a->right, b);

        int leftDistance = a->left == nullptr ? -1 : a->left->distance;
        int rightDistance = a->right == nullptr ? -1 : a->right->distance;

        if(rightDistance > leftDistance) {
            std::swap(a->left, a->right);
        }

        a->distance = a->right == nullptr ? 0 : a->right->distance + 1;

        return a;
    }

    static void Insert(LeftBiasTree*& a, int x) {
        LeftBiasTree* b = GetNewTree(x);
        a = Merge(a, b);
    }

    static int Pop(LeftBiasTree*& a) {
        int ret = a->value;
        a = Merge(a->left, a->right);
        return ret;
    }

    static void Init(LeftBiasTree* a) {
        a->value = 0;
        a->distance = 0;
        a->left = nullptr;
        a->right = nullptr;
    }

    static LeftBiasTree* GetNewTree() {
        auto ret = &Pool[PoolTail++];
        Init(ret);
        return ret;
    }

    static LeftBiasTree* GetNewTree(int x) {
        auto ret = &Pool[PoolTail++];
        Init(ret);
        ret->SetValue(x);
        return ret;
    }

    int GetValue() {
        return value;
    }

    void SetValue(int outValue) {
        value = outValue;
    }
};

LeftBiasTree Pool[MaxN << 2];
int father[MaxN];
LeftBiasTree* root[MaxN];


int FindX(int father[], int x) {
    if(father[x] == x) {
        return x;
    } else {
        return father[x] = FindX(father, father[x]);
    }
}


int main() {

    int n;
    while(~scanf("%d", &n)) {
        PoolTail = 0;
        for(int i = 1; i <= n; i++) {
            father[i] = i;
            root[i] = LeftBiasTree::GetNewTree();
            int outValue;
            scanf("%d", &outValue);
            root[i]->SetValue(outValue);
        }
        int m;
        scanf("%d", &m);

        while(m--) {
            int a, b;
            scanf("%d%d", &a, &b);

            int fA = FindX(father, a);
            int fB = FindX(father, b);

            if(fA == fB) {
                printf("-1\n");
                continue;
            }

            int popA = LeftBiasTree::Pop(root[fA]) / 2;
            int popB = LeftBiasTree::Pop(root[fB]) / 2;
            LeftBiasTree::Insert(root[fA], popA);
            LeftBiasTree::Insert(root[fB], popB);
            father[fB] = fA;
            root[fA] = LeftBiasTree::Merge(root[fA], root[fB]);
            printf("%d\n", root[fA]->GetValue());
        }
    }
    return 0;
}
```
