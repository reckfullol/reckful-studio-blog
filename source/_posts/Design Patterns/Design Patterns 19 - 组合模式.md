---
title: Design Patterns 19 - 组合模式
date: 2018-02-21 20:21:36
categories: Design Patterns
---
# 组合模式

<!--more-->

组合模式, 将对象组合成树形结构以表示"部分-整体"的层次结构. 组合模式使得用户对单个对象和组合对象的使用具有一致性.

```cs
abstract class Component {
    protected string name;

    public Component(string name) {
        this.name = name;
    }
    public abstract void Add(Component component);
    public abstract void Remove(Component component);
    public abstract void Display(int depth);
}

class Leaf : Component {
    public Leaf(string name) : base(name) {}

    public override void Add(Component component) {
        // cannot add to a leaf
    }

    public override void Remove(Component component) {
        // cannot remove from a leaf
    }

    public override void Display(int depth) {
        // todo
    }
}

class Composite : Component {
    private List<Component> children = new List<Component>();

    public Composite(string name) : base (name) {}

    public override void Add(Component component) {
        children.Add(component);
    }

    public override void Remove(Component component) {
        children.Remove(component);
    }

    public override void Display(int depth) {
        // todo
    }
}

public static void Main(string agrs[]) {
    Composite root = new Composite("root");
    root.Add(new Leaf("Leaf A"));
    root.Add(new Leaf("Leaf B"));

    Composite composite = new Composite("Composite X");
    composite.Add(new Leaf("Leaf XA"));
    composite.Add(new Leaf("Leaf XB"));

    root.Add(composite);
    root.Add(new Leaf("Leaf C"));

    root.Display();
}
```

## 透明模式与安全模式

- 透明模式, 在Component中声明所有用来管理子对象的方法, 其中包括Add, Remove等, 这样实现Component接口的所有子类都具备了Add和Remove. 这样做的好处就是叶节点和枝节点对于外界没有区别, 他们具备完全一致的行为接口. 但问题也很明显, 因为Leaf类本身不具备Add(), Remove()方法的功能, 所以实现他是没有意义的.
- 安全模式, 也就是在Component接口中不去声明Add和Remove方法, 那么子类的Leaf也就不需要去实现它, 而是在Composite声明所有用来管理子类对象的方法, 这样做就不会出现刚才提到的问题, 不过由于不够透明, 所以树叶和树枝类将不具有相同的接口, 客户端的调用需要做相应的判断, 带来了不便.

## 何时使用组合模式

需求中是体现部分与整体层次的结构时, 希望用户可以忽略组合对象与单个对象的不同, 统一的使用组合结构中的所有对象时, 就应该考虑用组合模式.