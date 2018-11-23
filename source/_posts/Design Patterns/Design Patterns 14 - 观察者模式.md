---
title: Design Patterns 14 - 观察者模式
date: 2018-02-21 09:08:44
categories: Design Patterns
---
# 观察者模式

<!--more-->

观察者模式又称发布-订阅模式, 定义了一种一对多的依赖关系, 让多个观察者对象同时监听某一个主题对象. 这个主题对象在状态发生变化时, 会通知所有观察者对象, 让他们能够自动更新自己.

```cs
abstract class Subject {
    private IList<Observer> observers = new List<Observer>();

    public void Attach(Observer observer) {
        observers.Add(observer);
    }

    public void Detach(Observer observer) {
        observers.Remove(observer);
    }

    public void Notify() {
        foreach(Observer observer in observers) {
            observer.Update();
        }
    }
}

abstract class Observer {
    public abstract void Update();
}

class ConcreteSubject : Subject {
    private string subjectState;

    public string SubjectState {
        get {
            return subjectState;
        }
        set {
            subjectState = value;
        }
    }
}

class ConcreteObserver : Observer {
    private string name;
    private string observerState;
    private ConcreteSubject subject;

    public ConcreteObserver(ConcreteSubject subject, string name) {
        this.subject = subject;
        this.name = name;
    }

    public override void Update() {
        observerState = subject.SubjectState();
        // todo
    }

    public ConcreteSubject Subject {
        get {
            return subject;
        }
        set {
            subject = value;
        }
    }
}

public static void Main(string[] agrs) {
    ConcreteSubject subject = new ConcreteSubject();

    subject.Attach(new ConcreteObserver(subject, "X"));
    subject.Attach(new ConcreteObserver(subject, "Y"));

    subject.SubjectState = "ABC";
    subject.Notify();
}
```

## 观察者模式特点

将一个系统分割成一系列相互协作的类有一个很不好的副作用, 那就是需要维护相关对象间的一致性, 我们不希望为了维持一致性而使各类紧密耦合, 这样会给维护, 扩展和重用都带来不便. 而观察者模式的关键对象是主题Subject和观察者Observer, 一个Subject可以有任意数目的依赖他的Observer, 一旦Subject的状态发生改变, 所有的Observer都可以得到通知.

当一个对象的改变需要同时改变其他对象, 而且他不知道具体有多少对象有待改变时, 应该考虑使用观察者模式.

观察者模式所做的工作其实就是在解耦合, 让耦合的双方都依赖于抽象, 而不是依赖于具体. 从而使得各自的变化都不会影响另一边的变化.

## 事件委托

委托就是一种引用方法的类型. 一旦为委托分配了方法, 委托将与该方法具有完全相同的行为. 委托方法的使用可以像其他任何方法一样, 具有参数和返回值, 委托可以看作是对函数的抽象, 是函数的类, 委托的实例将代表一种具体的函数.

一个委托可以搭载多个方法, 所有方法被一次唤起, 他可以使得委托对象所打在的方法并不属于同一个类.

委托的前提就是委托对象所搭载的所有方法必须具有相同的原型和形式, 也就是拥有相同的参数列表和返回值类型.

```cs
deleget void EventHandler();

class Subject() {
    public event EventHandler Update;

    public void Notify() {
        Update();
    }
}

public static void Main(string[] agrs) {
    Subject subject = new Subject();

    subject.Update += new EventHandler(objectA.MethodA);
    subject.Update += new EventHandler(objectB.MethodB);

    subject.Notify();
}
```