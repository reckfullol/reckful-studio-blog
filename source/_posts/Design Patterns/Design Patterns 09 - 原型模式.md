---
title: Design Patterns 09 - 原型模式
date: 2018-02-19 14:48:32
categories: Design Patterns
---
# 原型模式

<!--more-->

原型模式用原型实例指定创建对象的种类, 并且通过拷贝这些原型创建新的对象. 也就是从一个对象再创建另一个可定制的对象, 而且不需要知道任何创建的细节.

```cs
abstract class Prototype {
    private string id;

    public Prototype(string id) {
        this.id = id;
    }

    public string Id {
        get {
            return id;
        }
    }

    public abstract Prototype Clone();
}

class ConcretePrototype1 : Prototype {
    public ConcretePrototype1(string id) : base(id) {

    }

    public override Prototype Clone() {

        /* 
        创建当前对象的浅表副本. 方法是创建一个新对象, 
        然后将当前对象的非静态字段复制到该新对象. 如果字段是值类型的, 
        则对该字段执行逐位复制. 如果字段是引用类型, 
        则复制引用但不复制引用的对象: 因此, 原始对象及其副本引用同一对象.
        */
        return (Prototype) this.MemBerwiseClone();
    }
}

public static void Main(string[] agrs) {
    ConcretePrototype1 p1 = new ConcretePrototype1("I");
    ConcretePrototype1 c1 = new (ConcretePrototype1)p1.Clone();
}
```

对于.NET而言, 在System命名空间中提供了ICloneable接口, 其中唯一的一个方法就是Clone(), 因此只需要实现这个接口就能完成原型模式.