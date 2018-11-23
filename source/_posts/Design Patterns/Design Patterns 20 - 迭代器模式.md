---
title: Design Patterns 20 - 迭代器模式
date: 2018-02-21 21:58:11
categories: Design Patterns
---
# 迭代器模式

<!--more-->

迭代器模式, 提供一种方法顺序访问一个聚合对象中各个元素, 而又不暴露该对象的内部表示.

```cs
abstract class Iterator {
    public abstract object First();
    public abstract object Next();
    public abstract bool IsDone();
    public abstract object CurrentItem();
}

abstract classs Aggregate {
    public abstract Iterator CreateIterator();
}

class ConcreteTerator : Iterator {
    private ConcreteAggregate aggregate;
    private int current = 0;

    public ConcreteIterator(ConcreteAggregate aggregate) {
        this.aggregate = aggregate;
    }

    public override object First() {
        return aggregate[0];
    }

    public override object Next() {
        object ret = null;
        current++;
        if(current < aggregate.Count) {
            ret = aggregate[current];
        }
        retrun ret;
    }

    public override bool IsDone() {
        return current >= aggregate.Count;
    }

    public override object CurrentItem() {
        return aggregate[current];
    }
}

class ConcreteAggregate : Aggregate {
    private IList<object> items = new IList<Object>();

    public override Iterator CreateIterator() {
        return new ConcreteIterator(this);
    }

    public int Count {
        get {
            return items.Count;
        }
    }

    public object this[int index] {
        get {
            return items[index];
        }

        set {
            items.insert(index, value);
        }
    }
}
```