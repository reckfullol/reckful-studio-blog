---
title: Design Patterns 02 - 策略模式
date: 2018-02-07 22:52:17
categories: Design Patterns
---
# 策略模式

<!--more-->

面向对象的编程, 并不是类越多越好, 类的划分是为了封装, 但分类的基础是抽象, 具有相同属性和功能的对象的抽象集合才是类.

简单工厂只是解决对象的创建问题, 而且由于工厂本身包括了所有的方式, 每次维护或者扩展方式的时候都要改动这个工厂, 以至代码需要重新编译部属.

## 策略模式

策略模式定义了算法家族, 分别封装起来, 让他们之间可以互相替换, 此模式让算法变化, 不会影响到使用算法的用户.

```cs
abstract class Strategy {
    public abstract void AlgorithmInterface();
}
class  ConcreteStrategyA : Strategy {
    public override void AlgorithmInterface() {
        // todo
    }
}
class Context {
    Strategy strategy = null;
    public Context(Strategy strategy) {
        this.strategy = strategy;
    }
    public void ContextInterface() {
        strategy.AlgorithmInterface();
    }
}
public static void Main(string[] agrs) {
    Context context = new Context(new ConcreteStrategyA());
    context.ContextInterface();
}
```

## 策略与简单工厂结合

```cs
class Context {
    Strategy strategy = null;
    public Context(string type) {
        switch(type) {
            case "A" :
                strategy = new ConcreteStrategyA();
                break;
            default:
                break;
        }
    }
    public void ContextInterface() {
        strategy.AlgorithmInterface();
    }
}
public static void Main(string[] args) {
    Context context = new Context(kind);
    context.ContextInterface();
}
```

简单工厂模式客户端需要认识两个类, 而策略模式与简单工厂结合的用法, 客户端就只需要认识一个类Context就可以了.耦合更加降低.

## 策略模式解析

{% blockquote %}
策略模式是一种定义一系列算法的方法, 从概念上来看, 所有这些算法完成的都是相同的工作, 只是实现不同, 它可以以相同的方式调用所有的算法, 减少了各种算法类与使用算法类之间的耦合.

策略模式的Strategy类层次为Context定义了一系列的可供重用的算法或行为. 继承有助于析取出这些算法中的公共功能.

策略模式可以简化单元测试, 因为每个算法都有自己的类, 可以通过自己的接口单独测试.

当不同的行为堆砌在一个类中时, 就很难避免使用条件语句来选择合适的行为. 将这些行为封装在一个个独立的Strategy类中, 可以在使用这些行为的类中消除条件语句.
{% endblockquote %}