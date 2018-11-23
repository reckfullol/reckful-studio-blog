---
title: Design Patterns 01 - 简单工厂模式
date: 2018-02-07 21:52:17
categories: Design Patterns
---
# 简单工厂模式

<!--more-->

## 活字印刷之于面向对象

- 要改, 只需改要改之字, 此为可维护
- 活字可以在后来的印刷中重复使用, 此为可复用
- 若要加字, 只需另刻字, 此为可扩展
- 只需将活字移动就可满足排列需求, 此为高灵活性

## 封装

以计算器程序举例, 要将业务逻辑和界面逻辑分离, 使耦合度降低, 才能方便后期的维护和扩展.

```cs
public class Operation {
    public static double GetResult(double, double, string) {
        //todo
    }
}
public static void Main(string[] args) {
    Console.Write(GetResult(a, b, op));
}
```

## 继承 & 多态

如果要在Operation类中加入sqrt方法, 需要将其他运算都参与编译, 所以应该将各种运算分离.

```cs
public class Operation {
    private double _numberA;
    private double _numberB;
    public double NumberA {
        get {
            return _numberA;
        };
        set {
            _numberA = value;
        };
    }
    public double NumberB {
        get {
            return _numberB;
        };
        set {
            _numberB = value;
        };
    }

    public abstract double GetResult();
}
class OperationAdd : Operation {
    public override double GetResult() {
        return NumberA + NumberB;
    }
}
```

## 简单工厂模式

通过一个单独的类来创造实例, 这就是工厂.

```cs
public class OperationFactory {
    public static Operation CreateOperation(string operator) {
        Operation operation = null;
        switch(operator) {
            case "+" :
                operation = new OperationAdd();
                break;
            default:
                break;
        }
        return operation;
    }
}

public static void Main(string[] args) {
    Operation operation = OperationFactory.CreateOperation("+");
}
```