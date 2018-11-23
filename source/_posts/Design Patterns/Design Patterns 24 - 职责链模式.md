---
title: Design Patterns 24 - 职责链模式
date: 2018-02-22 14:27:48
categories: Design Patterns
---
# 职责链模式

<!--more-->

职责链模式, 使多个对象都有机会处理请求, 从而避免请求的发送者和接受者之间的耦合关系. 将这个对象连成一条链, 并沿着这条链传递该请求, 直到有一个对象处理为止.

```cs
abstract class Handler {
    protected Handler successor;

    public void SetSuccessor(Handler successor) {
        this.successor = successor;
    }

    public abstract void HandlerRequest(int request);
}

class ConcreteHandler1 : Handler {
    public override void HandlerRequest(int request) {
        if(request >= 0 && request < 10) {
            // todo
        }
        else if(successor != null) {
            successor.HandlerRequest(request);
        }
    }
}

class ConcreteHandler2 : Handler {
    public override void HandlerRequest(int request) {
        if(request >= 10 && request < 20) {
            // todo
        }
        else if(successor != null) {
            successor.HandlerRequest(request);
        }
    }
}

public void Main(string[] agrs) {
    Handler handler1 = new ConcreteHandler1();
    Handler handler2 = new ConcreteHandler2();
    handler1.SetSuccessor(handler2);

    int[] requests = {2, 5, 14, 19};

    foreach(int request in requests) {
        handler1.HandlerRequest(request);
    }
}
```

接受者和发送者都没有对方的明确信息, 且链中的对象自己也并不知道链的结构. 结构是职责链可简化对象的相互连接, 他们仅需保持一个指向其后继者的引用, 而不需保持他所有的候选接受者的引用.