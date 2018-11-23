---
title: Design Patterns 21 - 单例模式
date: 2018-02-22 12:36:32
categories: Design Patterns
---
# 单例模式

<!--more-->

单例模式, 保证一个类仅有一个实例, 并提供一个访问他的全局访问点.

```cs
class Singleton {
    private static Singleton instance;

    private Singleton() {

    }

    public static Singleton GetInstance() {
        if(instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

## 多线程时的单例

```cs
class Singleton {
    private static Singleton instance;
    private static readonly object syncRoot = new object();

    private Singleton() {

    }

    public static Singleton GetInstance() {
        lock(syncRoot) {
            if(instance == null) {
                instance = new Singleton();
            }
        }
        return instance;
    }
}
```

这样可以保证不会有多个实例的生成, 但是在每次调用GetInstance的时候都会调用lock, 会影响性能.

## 双重锁定

```cs
class Singleton {
    private static Singleton instance;
    private static readonly object syncRoot = new Object();

    private Singleton() {

    }

    public static Singleton GetInstance() {
        if(instance == null) {
            lock(syncRoot) {
                if(instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

## 静态初始化

```cs
// sealed阻止发生派生, 而派生可能会增加实例
public sealed class Singleton {
    // 在第一次引用类的任何成员时创建实例
    // 公共语言运行库负责处理变量初始化
    private static readonly Singleton instance = new Singleton();
    private Singleton() {

    }
    public static Singleton GetSintance() {
        return instance;
    }
}
```