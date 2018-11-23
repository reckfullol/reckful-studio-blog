---
title: Design Patterns 18 - 备忘录模式
date: 2018-02-21 18:03:16
categories: Design Patterns
---
# 备忘录模式

<!--more-->

备忘录模式, 在不破坏封装型的前提下, 捕获一个对象的内部状态, 并在该对象之外保存这个状态. 这样以后就可将该对象恢复到原先保存的状态.

```cs
class Originator {
    private string state;
    public string State {
        get {
            return state;
        }
        set {
            state = value;
        }
    }

    public Memento CreateMemento {
        return (new Memento(state));
    }

    public void SetMemento(Memento memento) {
        state = memento.State;
    }

    public void Show() {
        // todo
    }
}

class Memento {
    private string state;

    public Memento(string state) {
        this.state = state;
    }

    public string State {
        get {
            return state;
        }
    }
}

class Caretaker {
    private Memento memento;

    public Memento Memento {
        get {
            return memento;
        }
        set {
            memento = value;
        }
    }
}

public static void Main(string[] args) {
    Originator originator = new Originator();
    originator.State = "On";
    originator.Show();

    Caretaker caretaker = new Caretaker();
    caretaker.Memento = originator.CreateMemento();

    originator.State = "Off";
    originator.Show();

    originator.SetMemento(caretaker.Memento);
    originator.Show();
}
```