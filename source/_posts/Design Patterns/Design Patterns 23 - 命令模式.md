---
title: Design Patterns 23 - 命令模式
date: 2018-02-22 13:50:05
categories: Design Patterns
---
# 命令模式

<!--more-->

命名模式, 将一个请求封装为一个对象, 从而使你可用不同的请求对客户进行参数化; 对请求排队或记录请求日志, 以及支持可撤销的操作.

```cs
abstract class Command {
    protected Receiver receiver;

    public Command(Receiver receiver) {
        this.receiver = receiver;
    }

    abstract public void Execute();
}

class ConcreteCommand :  Command {
    public ConcreteCommand(Receiver receiver) : base(receiver) {
    }

    public override void Execute() {
        receiver.Action();
    }
}

class Invoker {
    private Command command;

    public void SetCommand(Command command) {
        this.command = command;
    }

    public void ExecuteCommand() {
        command.Execute();
    }
}

class Receiver {
    public void Action {
        // todo
    }
}

public static void Main(string[] agrs) {
    Receiver receiver = new Receiver();
    Command command = new ConcreteCommand(receiver);
    Invoker invoker = new Invoker();
    invoker.SetCommand(command);
    invoker.ExecuteCommand();
}
```

## 命令模式作用

1. 他能较容易的设计一个命令队列.
2. 在需要的情况下, 可以较容易的将命令计入日志.
3. 允许接收请求的一方是否要否决请求.
4. 可以容易的实现对请求的撤销和重做.
5. 由于加进新的具体命令类不影响其他的类, 因此增加新的具体命令类很容易.
6. 命令模式把请求一个操作的对象与知道怎么执行一个操作的对象分隔开.

敏捷开发原则告诉我们, 不要为代码添加基于猜测的, 实际不需要的功能. 如果不清楚一个系统是否需要命令模式, 一般就不要着急去实现他, 事实上, 在需要的时候通过重构实现这个模式并不困难, 只有在真正需要如撤销/恢复操作等功能时, 把原来的代码重构为命令模式才有意义.