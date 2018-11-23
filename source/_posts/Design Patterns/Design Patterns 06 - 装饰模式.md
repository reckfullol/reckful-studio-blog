---
title: Design Patterns 06 - 装饰模式
date: 2018-02-18 12:28:53
categories: Design Patterns
---
# 装饰模式

<!--more-->

动态的给一个对象添加一些额外的职责, 就增加功能来说, 装饰模式比生成子类更为灵活.

```cs
abstract class Component {
    public abstract void Operation();
}

class ConcreteComponent : Component {
    public override void Operation() {
        // todo
    }
}

abstract class Decorator : Component {
    protected Component component;

    public void SetComponent(Component component) {
        this.component = component;
    }

    public override void Operation() {
        if(component != null) {
            component.Operation();
        }
    }
}

class ConcreteDecoratorA : Decorator {
    private string addedState;

    public override void Opertaion() {
        base.Operation();
        addedState = "New State";
        // todo
    }
}

class ConcreteDecoratorB : Decorator {
    public override void Operation() {
        base.Operation();
        AddedBehavior();
        // todo
    }

    private void AddedBehavior() {
        // todo
    }
}

public static void Main(string[] args) {
    ConcreteComponent concreteComponent = new ConcreteComponent();
    ConcreteDecoratorA concreteDecoratorA = new ConcreteDecoratorA(concreteComponent);
    ConcreteDecoratorB concreteDecoratorB = new ConcreteDecoratorB(concreteDecoratorA);
    concreteDecoratorB.Operation();
}
```

装饰模式是利用SetComponent来对对象进行包装, 这样每个装饰对象的实现就和如何使用这个对象分离开了, 每个装饰对象只关心自己的功能, 就不需要关心如何被添加到对象链当中.

装饰模式的优点是有效的把类的核心职责和装饰功能区分开了, 而且可以去除相关类中重复的装饰逻辑.