---
title: Design Patterns 22 - 桥接模式
date: 2018-02-22 13:01:51
categories: Design Patterns
---
# 桥接模式

<!--more-->

对象的继承关系是在编译时就定义好了, 所以无法在运行时改变从父类继承的实现, 子类的实现与他的父类有非常紧密的依赖关系, 以至于父类实现中的任何变化必然会导致子类发生变化. 当你需要复用子类时, 如果继承下来的实现不适合解决新的问题, 则父类必须重写或被其他更适合的类替换. 这种依赖关系限制了灵活性并最终限制了服用性.

## 合成/聚合服用原则

合成/聚合服用原则, 尽量使用合成/聚合, 尽量不要使用类继承. 聚合表示一种弱的"拥有"关系, 体现的是A对象可以包含B对象, 但B对象不是A对象的一部分; 合成则是一种强的"拥有"关系, 体现了严格的部分和整体关系, 部分和整体的生命周期一样.

优先使用对象的合成/聚合将有助于你保持每个类被封装, 并被集中在单个任务上. 这样类和类继承层次会保持较小规模, 并且不太可能增长为不可控制的庞然大物.

## 桥接模式

桥接模式, 将抽象部分与他的实现部分分离, 使他们都可以独立的变化. 抽象与他的实现分离, 这并不是说, 让抽象类与其派生类分离, 因为这没有任何意义. 实现指的是抽象类和他的派生类用来实现自己的对象.

```cs
abstract class Implementor {
    public abstract void Operation();
}

class ConcreteImplementorA : Implementor {
    public override void Operation() {
        // todo
    }
}
class ConcreteImplementorB : Implementor {
    public override void Operation() {
        // todo
    }
}

class Abstraction {
    protected Implementor implementor;

    public void SetImplementor(Implementor implementor) {
        this.implementor = implementor;
    }

    public virtual void Operation() {
        implementor.Operation();
    }
}

class RefinedAbstraction : Abstraction {
    public override void Operation() {
        implementor.Operation();
    }
}

public static void Main(string[] agrs) {
    Abstraction abstraction = new RefinedAbstraction();

    abstraction.SetImplementor(new ConcreteImplementorA());
    abstraction.Operation();

    abstraction.SetImplementor(new ConcreteImplementorB());
    abstraction.Operation();
}
```

实现系统可能有多角度分类, 每一种分类都有可能变化, 那么就把这种多角度分离出来让他们独立变化, 减少他们之间的耦合.