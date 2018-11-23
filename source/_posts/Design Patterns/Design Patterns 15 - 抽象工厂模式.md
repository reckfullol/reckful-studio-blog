---
title: Design Patterns 15 - 抽象工厂模式
date: 2018-02-21 10:03:53
categories: Design Patterns
---
# 抽象工厂模式

<!--more-->

抽象工厂模式, 提供一个创建一系列相关或相互依赖对象的接口, 而无需指定他们具体的类.

## 抽象工厂的优点与缺点

- 优点
    1. 易于交换产品系列, 由于具体工厂类, 例如IFactory factory = new AccessFactory(), 在一个应用中只需要初始化的时候出现一次, 这就使得改变一个应用的具体工厂变得非常容易, 他只需要改变具体工厂即可使用不同的产品配置.
    2. 他让具体的创建实例过程与客户端分离, 客户端是用过他们的抽象接口操纵实例, 产品的具体类名也被具体工厂的实现分离, 不会出现在客户代码中.
- 缺点
    1. 如果需求来自增加功能, 即增加方法, 那么对于每一种产品系列都要添加相应方法及其抽象类, 同时对于每种工厂类和抽象工厂类都要进行改动.
    2. 每一个类的开始都需要声明IFactory factory = AccessFactory(), 那么假如有100个调用该工厂的类, 在需要改动的时候要改动100次.

## 反射+抽象工厂

反射格式:
```cs
Assembly.Load("程序集名称").CreateInstance("命名空间.类名称")
```

```cs
using System.Reflection;

IUser result = (IUser)Assembly.Load("抽象工厂模式").CreateInstance("抽象工厂模式".AccessUser);
```

之前选择产品类型的工厂是通过在编译前就决定的, 而反射通过字符串来实例化对象, 可以用变量来处理, 根据需要更换.

```cs
using System.Reflection;

class DataAccess {
    private static readonly string AssemblyName = "抽象工厂模式";
    private static readonly string db = "Sqlserver";

    public static IUser CreateUser() {
        string className = AssemblyName + "." + db + "User";
        return (IUser)Assembly.Load(AssemblyName).CreateInstance(className);
    }

    public static IDepartment CreateDepartment() {
        string className = AssemblyName + "." + db + "Department";
        return (IDepartment)Assembly.Load(AssemblyName).CreateInstance(className);
    }
}
```

但是在更换数据库访问时, 我们还是需要去改程序(更改db这个字符串的值)重编译, 不符合开放-封闭原则.

## 反射+配置文件

利用配置文件给DB字符串赋值.

```xml
<?xml version = "1.0" encoding = "utf=8" ?>
<configuration>
    <appSettings>
        <add key = "DB" value = "Sqlserver"/>
    </appSettings>
</configuration>
```

所有在用简单工厂的地方, 都可以考虑用反射技术来去除switch或if, 解除分支判断带来的耦合.