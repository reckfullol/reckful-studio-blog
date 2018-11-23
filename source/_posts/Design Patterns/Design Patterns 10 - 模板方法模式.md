---
title: Design Patterns 10 - 模板方法模式
date: 2018-02-19 23:48:10
categories: Design Patterns
---
# 模板方法模式

<!--more-->

模板方法模式, 定义一个操作中的算法的骨架, 而将一些步骤延迟到子类中. 模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤.

```cs
abstract class AbstractClass {
    public abstract void PrimitiveOperation1();
    public abstract void PrimitiveOperation2();

    public void TemplateMethod() {
        PrimitiveOperation1();
        PrimitiveOperation2();
    }
}
class ConcreteClassA : AbstractClass {
    public override void PrimitiveOperation1() {
        // todo
    }

    public override void PrimitiveOperation2() {
        // todo
    }
}

class ConcreteClassB : AbstractClass {
    public override void PrimitiveOperation1() {
        // todo
    }

    public override void PrimitiveOperation2() {
        // todo
    }
}

public static void Main(string[] agrs) {
    AbstractClass c;

    c = new ConcreteClassA();
    c.TemplateMethod();

    c = new ConcreteClassB();
    c.TemplateMethod();    
}
```

## 模板方法模式特点

模板方法模式是通过把不变行为搬移到超类, 去除子类中的重复代码来体现他的优势. 这样就帮助子类摆脱重复的不变行为的纠缠.