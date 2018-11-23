---
title: Design Patterns 08 - 工厂方法模式
date: 2018-02-19 13:40:55
categories: Design Patterns
---
# 工厂方法模式

<!--more-->

工厂方法模式定义了一个用于创建对象的接口, 让子类决定实例化哪一个类. 工厂方法使一个类的实例化延迟到其子类.

简单工厂模式的最大优点在于工厂类中包含了必要的逻辑判断, 根据客户端的选择条件动态实例化相关的类, 对于客户端来说, 去除了与具体产品的依赖. 但是这违背了开放-封闭原则, 不但对扩展开放了, 对修改也开放了.

```cs
interface Creator {
    Product FactoryMethod();
}
class ConcreteCreator : Creator {
    public override FactoryMethod() {
        // todo
    }  
}
interface Product {
    // todo
}
class ConcreteProduct : Product {
    // todo
}
```

工厂方法把简单工厂的内部逻辑判断移到了客户端代码来进行.