---
title: Java 16 - 序列化与反序列化
date: 2018-07-29 03:10:03
categories: Java
---
# 序列化与反序列化

<!--more-->

序列化是只将对象转化为字节流.

序列化通过: ObjectOutputStream.writeObject().

反序列化通过: ObjectInputStream.readObject().

需要进行序列化的类需要实现Serializable接口, 但是不需要实现任何方法.

注意是的序列化并不会对静态变量起作用, 因为序列化只是保存对象的状态, 而不是类的状态.

通过添加transient关键字可以阻止对对象中的字段序列化, 使其值为null.

```java
import java.io.*;

public class Test {
    static class A implements Serializable {
        private int x;
        private String y;

        A(int x, String y) {
            this.x = x;
            this.y = y;
        }

        @Override
        public String toString() {
            return "x = " + x + " " + "y = " + y;
        }
    }
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        A a1 = new A(123, "abc");
        String objectFile = "./a1";
        ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(objectFile));
        objectOutputStream.writeObject(a1);
        objectOutputStream.close();

        ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream(objectFile));
        A a2 = (A) objectInputStream.readObject();
        objectInputStream.close();
        System.out.println(a2);
    }

}
```