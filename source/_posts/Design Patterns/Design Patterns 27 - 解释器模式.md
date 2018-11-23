---
title: Design Patterns 27 - 解释器模式
date: 2018-02-24 14:23:19
categories: Design Patterns
---
# 解释器模式

<!--more-->

解释器模式, 给定一个语言, 定义他的文法的一种表示, 并定义一个解释器, 这个解释器使用该表示来解释语言中的句子.

如果一种特定类型的问题发生的频率足够高, 那么可能就值得将该问题的各个实例表述为一个简单语言中的句子. 这样就可以构建一个解释器, 该解释器通过解释这些句子来解决该问题.

```cs
abstract class AbstractExpression {
    public abstract void Interpret(Context context);
}

class TerminalExpression : AbstractExpression {
    public override void Interpret(Context) {
        // todo
    }
}

class NonTerminalExpression : AbstractExpression {
    public override void Interpret(Context context) {
        // todo
    }
}

class Context {
    private string input;
    private string output;

    public string Input {
        get {
            return input;
        }
        set {
            input = value;
        }
    }

    public string Output {
        get {
            return output;
        }
        set {
            output =  value;
        }
    }
}

public static void Main(string[] agrs) {
    Context context = new Context();
    IList<AbstractExpression> list = new List<AbstractExpression>
    list.Add(new TerminalExpression());
    list.Add(new NonTerminalExpression());

    foreach(var expression in list) {
        expression.Interpret(context);
    }
}
```

## 解释器模式好处

当有一个语言需要解释执行, 并且你可将该语言中的句子表示为一个抽象语法树时, 可使用解释器模式.

解释器模式可以很容易的改变和扩展文法, 因为该模式使用类来表示文法规则, 你可使用继承来改变或扩展该文法. 该比较容易实现文法, 因为定义抽象语法树中各个节点的类的实现大体类似, 这些类都易于直接编写.

## 解释器模式不足

解释器模式为文法中的每一条规则至少定义了一个类, 因此包含许多规则的文法可能难以管理和维护. 建议当文法非常复杂时, 使用其他的技术如语法分析程序或编译器生成器来处理.