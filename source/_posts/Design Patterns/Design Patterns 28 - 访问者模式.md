---
title: Design Patterns 28 - 访问者模式
date: 2018-02-24 15:22:41
categories: Design Patterns
---
# 访问者模式

<!--more-->

访问者模式, 表示一个作用与某对象结构中的各元素的操作. 它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作.

```cs
abstract class Visitor {
    public abstract void VisitConcreteElementA(ConcreteElementA concreteElementA);
    public abstract void VisitConcreteElementB(ConcreteElementB concreteElementB);    
}

class ConcreteVisitor1 : Visitor {
    public override void VisitConcreteElementA(ConcreteElementA concreteElementA) {
        // todo
    }
    public override void VisitConcreteElementB(ConcreteElementB concreteElementB) {
        // todo
    }
}

class ConcreteVisitor2 : Visitor {
    public override void VisitConcreteElementA(ConcreteElementA concreteElementA) {
        // todo
    }
    public override void VisitConcreteElementB(ConcreteElementB concreteElementB) {
        // todo
    }
}

abstract class Element {
    public abstract void Accept(Visitor visitor);
}

class ConcreteElementA : Element {
    public override void Accept(Visitor visitor) {
        visitor.VisitConcreteElementA(this);
    }
    public void OperationA {
        // todo
    }
}

class ConcreteElementB : Element {
    public override void Accept(Visitor visitor) {
        visitor.VisitConcreteElementB(this);
    }
    public void OperationB {
        // todo
    }
}

class ObjectStructure {
    private IList<Element> elements = new List<Element>();

    public void Attach(Element element) {
        elements.Add(element);
    }

    public void Detach(Element element) {
        elements.Remove(element);
    }

    public void Accept(Visitor visitor) {
        foreach(Element element in elements) {
            element.Accept(visitor);
        }
    }
}

public static void Main(stringp[] agrs) {
    ObjectStructure objectStructure = new ObjectStructure();
    objectStructure.Attach(new ConcreteElementA());
    objectStructure.Attach(new ConcreteElementB());

    ConcreteVisitor1 concreteVisitor1 = new ConcreteVisitor1();
    ConcreteVisitor2 concreteVisitor2 = new ConcreteVisitor2();

    objectStructure.Accept(concreteVisitor1);
    objectStructure.Accept(concreteVisitor2);
}
```