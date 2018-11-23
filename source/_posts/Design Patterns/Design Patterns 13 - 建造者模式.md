---
title: Design Patterns 13 - 建造者模式
date: 2018-02-20 12:18:39
categories: Design Patterns
---
# 建造者模式

<!--more-->

建造者模式, 将一个复杂对象的构建与他的表示分离, 使得同样的构建过程可以创建不同的表示.

```cs
class Product {
    IList<string> parts = new List<string>();

    public void Add(string part) {
        parts.Add(part);
    }

    public void Show() {
        foreach(string part in parts) {
            // todo
        }
    }
}

abstract class Builder {
    public abstract void BuildPartA();
    public abstract void BuildPartB();
    public abstract void Product GetResult();
}

class ConcreteBuilder1 : Builder {
    private Product product = new Product();

    public override void BuildPartA() {
        product.Add("part A");
    }

    public override void BuildPartB() {
        product.Add("part B");
    }

    public override Product GetResult() {
        return product;
    }
}

class ConcreteBuilder2 : Builder {
    private Product product = new Product();

    public override void BuildPartA() {
        product.Add("part X");
    }

    public override void BuildPartB() {
        product.Add("part Y");
    }

    public override Product GetResult() {
        return product;
    }
}

class Director {
    public void Construct(Builder builder) {
        builder.BuilderA();
        builder.BuilderB();
    }
}

public static void Main(string[] agrs) {
    Director director = new Director();
    Builder b1 = new ConcreteBuilder1();
    Builder b2 = new ConcreteBuilder2();

    director.Construct(b1);
    Product p1 = b1.GetResult();
    p1.Show();

    director.Construct(b2);
    Product p2 = b2.GetResult();
    p2.Show();
}
```

建造者模式的好处就是使得建造代码与表示代码分离, 由于建造者隐藏了该产品是如何组装的, 所以若需要改变一个产品的内部表示, 只需要再定义一个具体的建造者就可以了.