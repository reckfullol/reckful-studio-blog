---
title: Design Patterns 26 - 享元模式
date: 2018-02-24 14:21:51
categories: Design Patterns
---
# 享元模式

<!--more-->

享元模式, 运用共享技术有效的支持大量细粒度的对象.

```cs
abstract class Flyweight {
    public abstract void Operation(int extrinsicstate);
}

class ConcreteFlyweight : Flyweight {
    public override void Operation(int extrinsicstate) {
        // todo
    }
}

class UnsharedConcreteFlyweight : Flyweight {
    public override void Operation(int extrinsicstate) {
        // todo
    }
}

class FlyweightFactory {
    private Hashtable flyweights = new Hashtable();

    public FlyweightFactory() {
        flyweights.Add("X", new ConcreteFlyweight());
        flyweights.Add("Y", new ConcreteFlyweight());
        flyweights.Add("Z", new ConcreteFlyweight());
    }

    publiuc Flyweight GetFlyweight(string key) {
        return ((Flyweight)flyweights[key]);
    }
}
```

## 内部状态与外部状态

在享元对象内部并且不会随环境变化而改变的共享部分, 可以称为是享元对象的内部状态, 而随环境改变的改变的, 不可以共享的状态就是外部状态了. 享元模式可以避免大量非常相似类的开销. 在程序设计中, 有时需要生成大量细粒度的类实例来表示数据. 如果能发现这些实例除了几个参数外基本上都是相同的, 有时就能够大幅度的减少需要实例化的类的数量. 如果能把那些参数移到类实例的外面, 在方法调用时将他们传递进来, 就可以通过共享大幅度的减少单例的数目.

## 享元模式应用

如果一个应用程序使用了大量的对象, 而大量的这些对象造成了很大的存储开销时就应该考虑使用; 还有就是对象的大多数状态可以是外部状态, 如果删除对象的外部状态, 那么可以用相对较少的共享对象取代很多组对象, 此时可以考虑使用享元模式.