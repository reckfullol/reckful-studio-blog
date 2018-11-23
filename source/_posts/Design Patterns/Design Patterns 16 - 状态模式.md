---
title: Design Patterns 16 - 状态模式
date: 2018-02-21 11:18:36
categories: Design Patterns
---
# 状态模式

<!--more-->

状态模式, 当一个对象的内在状态改变时允许改变其行为, 这个对象看起来像是改变了其类.

状态模式主要解决的是当控制一个对象状态转换的条件表达式过于复杂时的状况. 把状态的判断逻辑转移到表示不同状态的一系列类中, 可以把复杂的判断逻辑简化.

```cs
abstract class State {
    public abstract void Handle(Context context);
}

class ConcreteStateA : State {
    public override void Handle(Context context) {
        // todo
        
        // 设置ConcreteStateA的下一状态是ConcreteStateB
        context.State = new ConcreteStateB();
    }
}

class ConcreteStateB : State {
    public override void Handle(Context context) {
        // todo

        // 设置ConcreteStateB的下一状态是ConcreteStateA
        context.State = new ConcreteStateA();
    }
}

class Context {
    private State state;
    public Context(State state) {
        this.state = state;
    }

    public State State {
        get {
            return state;
        }
        set {
            state = value;
            // todo
        }
    }

    public void Request() {
        state.Handle(this);
    }
}

public static void Main(string[] agrs) {
    Context context = new Context(new ConcreteStateA());

    context.Request();
    context.Request();
    context.Request();
    context.Request();
}
```

## 状态模式好处与作用

1. 将与特定状态相关的行为局部化, 并且将不同状态的行为分隔开来.
2. 将特定的状态相关行为都放入一个对象中, 由于所有与状态相关的代码都存在于某个ConcreteState中, 所以通过定义新的子类可以很容易地增加新的状态和转换.
3. 状态模式的目的就是为了消除庞大的条件分支语句, 通过把各种状态转移逻辑分布到State的子类之间, 来减少相互间的依赖.
4. 当对象的行为取决于他的状态, 并且他必须在运行时刻根据状态改变他的行为时, 就可以考虑使用状态模式.