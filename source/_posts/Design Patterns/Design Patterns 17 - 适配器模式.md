---
title: Design Patterns 17 - 适配器模式
date: 2018-02-21 11:41:28
categories: Design Patterns
---
# 适配器模式

<!--more-->

适配器模式, 将一个类的接口转换成客户希望的另一个接口. Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作.

系统的数据和行为都正确, 但接口不符时, 我们应该考虑用适配器, 目的是使控制范围之外的一个原有对象与某个接口匹配. 适配器模式主要应用于希望复用一些现存的类, 但是接口又与复用环境要求不一致的情况.

在GoF的设计模式中, 对适配器模式讲了两种类型, 类适配器模式和对象适配器模式, 由于类适配器模式通过多重继承对一个接口与另一个接口进行匹配, 而C#, JAVA等语言都不支持多重继承, 所以这里主要写对象适配器.

```cs
class Target {
    public virtual void Request() {
        // todo
    }
}

class Adaptee {
    public void SpecificRequest() {
        // todo
    }
}

class Adapter : Target {
    private Adaptee adaptee = new Adaptee();

    public override void Request() {
        adaptee.SpecificRequest();
    }
}

public static void Main(string[] args) {
    Target target = new Adapter();
    target.Request();
}
```