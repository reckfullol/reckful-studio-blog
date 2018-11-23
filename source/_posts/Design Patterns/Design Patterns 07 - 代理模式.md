---
title: Design Patterns 07 - 代理模式
date: 2018-02-19 11:27:10
categories: Design Patterns
---
# 代理模式

<!--more-->

为其他对象提供一种代理来控制对这个对象的访问.

```cs
abstract class Subject {
    public abstract void Request();
}

class RealSubject : Subject {
    public override void Request() {
        // todo
    }
}

class Proxy : Subject {
    RealSubject realSubject;
    public override void Request() {
        if(realSubject == null) {
            realSubject = new RealSubject();
        }
        realSubject.Request();
    }
}

public static void Main(string[] args) {
    Proxy proxy = new Proxy();
    proxy.Request();
}
```

## 代理模式应用

1. 远程代理, 也就是为一个对象在不同的地址空间提供局部代表. 这样可以隐藏一个对象存在于不同地址空间的是事实.
2. 虚拟代理, 是根据需要创建开销很大的对象. 通过他来存放实例化需要很长时间的真实对象.
3. 安全代理, 用来控制真实对象访问时的权限.
4. 智能指引, 是指当调用真实的对象时, 代理处理另外一些事.