---
title: Design Patterns 25 - 中介者模式
date: 2018-02-22 14:46:02
categories: Design Patterns
---
# 中介者模式

<!--more-->

尽管将一个系统分割成许多对象通常可以增加其可服用性, 但是对象间相互连接的激增又会降低其可复用性了. 大量的连接使得一个对象不可能在没有改变其他对象的支持下工作, 系统表现为一个不可分割的整体, 所以, 对系统的行为进行任何较大的改动就十分困难了.

中介者模式, 用一个中介对象来封装一系列的对象交互. 中介者使各对象不需要显式的相互引用, 从而使其耦合松散, 而且可以独立的改变他们之间的交互.

```cs
abstract class Mediator {
    public abstract void Send(string message, Colleague colleague);
}

abstract class Colleague {
    protected Mediator mediator;

    public Colleague(Mediator mediator) {
        this.mediator = mediator;
    }
}

class ConcreteMediator : Mediator {
    private ConcreteColleague1 colleague1;
    private ConcreteColleague2 colleague2;

    public ConcreteColleague1 Colleague1 {
        set {
            colleague1 = value;
        }
    }
    public ConcreteColleague2 Colleague2 {
        set {
            colleague2 = value;
        }
    }

    public override void Send(string message, Colleague colleague) {
        if(colleague == colleague1) {
            colleague2.Notify(message);
        }
        else {
            colleague1.Notify(message);
        }
    }
}

class ConcreteColleague1 : Colleague {
    public ConcreteColleague1(Mediator mediator) : base(mediator) {

    }

    public void Send(string message) {
        mediator.Send(message, this);
    }

    public void Notify(string message) {
        // todo
    }
}

class ConcreteColleague2 : Colleague {
    public ConcreteColleague2(Mediator mediator) : base(mediator) {

    }

    public void Send(string message) {
        mediator.Send(message, this);
    }

    public void Notify(string message) {
        // todo
    }
}
```

## 中介者模式优缺点

中介者模式很容易在系统中应用, 也很容易在系统中误用. 当系统出现了"多对多"交互复杂的对象群时, 不要急于使用中介者模式, 而要先反思你的系统在设计上是不是合理.

- 优点
    1. Mediator的出现减少了各个Colleague的耦合, 使得可以独立的改变和复用各个Colleague类和Mediator.
    2. 由于把对象如何协作进行了抽象, 将中介作为一个独立的概念并将其封装在一个对象中,  这样关注的对象就从对象各自本身的行为转移到他们之间的交互上来, 也就是站在一个更宏观的角度去看待系统.
- 缺点
    1. 由于ConcreteMediator控制了集中化, 于是就把交互复杂性变为了中介者的复杂性, 这就使得中介者会变得比任何一个ConcreteColleague都复杂.

中介者模式一般应用与一组对象已定义良好但是复杂的方式进行通信的场合, 以及想定制一个分布在多个类中的行为, 而又不想生成太多的子类的场合.