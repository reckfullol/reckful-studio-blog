---
title: Design Patterns 12 - 外观模式
date: 2018-02-20 10:43:28
categories: Design Patterns
---
# 外观模式

<!--more-->

外观模式, 为子系统中的一组接口提供一个一致的界面, 此模式定义了一个高层接口, 这个接口使得这一子系统更加容易使用.

```cs
class SusSystemOne {
    public void MethodOne() {
        // todo
    }
}

class SubSystemTwo {
    public void MethodTwo() {
        // todo
    }
}

class SubSystemThree {
    public void MethodThree() {
        // todo
    }
}

class SubSystemFour {
    public void MethodFour() {
        // todo
    }
}

class Facade {
    SubSystemOne one;
    SubSystemTwo two;
    SubSystemThree three;
    SubSystemFour four;

    public Facade() {
        one = new SubSystemOne();
        two = new SubSystemTwo();
        three = new SubSystemThree();
        four = new SubSystemFour();
    }

    public void MethodA() {
        one.MethodOne();
        two.MethodTwo();
        four.MethodFour();
    }

    public void MethodB() {
        two.MethodTwo();
        three.MethodThree();
    }
}

public static void Main(string[] agrs) {
    Facade facade = new Facade();

    facade.MathodA();
    facade.MathodB();
}
```

## 何时使用外观模式

1. 在设计初期阶段, 应该要有意识的将不同的两个层分离, 层与层之间建立外观Facade.
2. 在开发阶段, 子系统往往因为不断的重构演化而变得越来越复杂, 增加外观Facade可以提供一个简单的接口, 减少他们之间的依赖.
3. 在维护一个遗留的大型系统时, 可能这个系统已经非常难以维护和扩展了, 为新系统开发一个外观Facade类, 来提供设计粗糙或者高度复杂的遗留代码的比较清晰简单的接口, 让新系统与Facade对象交互, Facade与遗留代码交互所有复杂的工作.