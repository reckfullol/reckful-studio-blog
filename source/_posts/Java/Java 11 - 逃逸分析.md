---
title: Java 11 - 逃逸分析
date: 2018-07-26 17:14:48
categories: Java
---
# 逃逸分析

<!--more-->

## 定义

逃逸分析是一种可以有效减少Java中同步负载和内存堆分配压力的跨函数全局数据流分析方法. 通过逃逸分析, 编译器能够分析出一个新的对象的引用范围, 从而决定是否要将这个对象分配在堆上.

逃逸分析是指分析指针动态范围的方法, 当变量或者对象在方法中被分配后, 其指针有可能被返回或者被返回引用. 那么我们把其指针被其他过程或者线程所引用的现象叫做指针(引用)的逃逸.

## 实现

通过连通图来描述对象和对象之间的可达性关系. 这个实现是基于"封闭世界"的前提, 即所有可能被执行的方法在做逃逸分析前都已经得知, 并且, 程序的实际运行不会改变他们之间的调用关系. 但是在Java实际运行的时候, 这样的假设并不成立. 因为Java中的很多特性, 比如动态类加载, 调用本地函数, 反射程序调用都将打破"封闭世界"的约定.

## 处理

逃逸分析之后, 可以得到三种对象的逃逸状态:

1. 全局逃逸(GlobalEscape): 一个对象的引用逃出了方法或者线程. 比如一个对象的引用赋值给了一个类变量, 或者存储在一个已经逃逸的对象中, 或者这个对象的引用作为方法的返回值给了调用方法.

2. 参数逃逸(ArgEscape): 方法调用过程中传递对象的方法给调用方. 这种状态可以通过被调方法的二进制码确定.

3. 没有逃逸(NoEscape): 一个可以进行标量替换的对象, 可以不将对象分配在传统的堆上.

编译器可以对逃逸分析的结果进行优化:

1. 堆分配对象变成栈分配对象. 一个方法中的对象, 对象引用没有用发生逃逸, 那么对象可能会被分配在占内存上.

2. 消除同步. 逃逸分析可以判断出某个对象是否始终被一个线程访问. 如果只被一个线程访问, 那么对该对象的同步操作就可以转化成没有同步保护的操作.

3. 矢量替代. 如果发现对象的内存存储结构不需要连续进行, 就可以将对象的部分甚至全部都保存在CPU的寄存器中.

## 比较

测试代码:

```java
public class Test {
    private static class Foo {
        private int x;
        private static int counter;

        Foo() {
            x = (++counter);
        }
    }

    public static void main(String[] args) {
        long start = System.nanoTime();
        for (int i = 0; i < 1000 * 1000 * 10; ++i) {
            Foo foo = new Foo();
        }
        long end = System.nanoTime();
        System.out.println("Time cost is " + (end - start));
    }
}
```

使用逃逸分析优化(-server -XX:+DoEscapeAnalysis -XX:+PrintGC)

```
[0.004s][warning][gc] -XX:+PrintGC is deprecated. Will use -Xlog:gc instead.
[0.027s][info   ][gc] Using G1
Time cost is 10643889 
```

不使用逃逸分析优化(-server -Xmx10m -Xms10m -XX:-DoEscapeAnalysis -XX:+PrintGC)

```
[0.005s][warning][gc] -XX:+PrintGC is deprecated. Will use -Xlog:gc instead.
[0.022s][info   ][gc] Using G1
[0.261s][info   ][gc] GC(0) Pause Young (G1 Evacuation Pause) 4M->1M(10M) 2.379ms
[0.266s][info   ][gc] GC(1) Pause Young (G1 Evacuation Pause) 3M->1M(10M) 2.136ms
[0.267s][info   ][gc] GC(2) Pause Young (G1 Evacuation Pause) 3M->1M(10M) 0.555ms
[0.268s][info   ][gc] GC(3) Pause Young (G1 Evacuation Pause) 4M->1M(10M) 0.302ms
[0.269s][info   ][gc] GC(4) Pause Young (G1 Evacuation Pause) 4M->1M(10M) 0.334ms
[0.271s][info   ][gc] GC(5) Pause Young (G1 Evacuation Pause) 5M->1M(10M) 0.337ms
[0.272s][info   ][gc] GC(6) Pause Young (G1 Evacuation Pause) 5M->1M(10M) 0.315ms
[0.273s][info   ][gc] GC(7) Pause Young (G1 Evacuation Pause) 6M->1M(10M) 0.352ms
[0.275s][info   ][gc] GC(8) Pause Young (G1 Evacuation Pause) 6M->1M(10M) 0.419ms
[0.277s][info   ][gc] GC(9) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.336ms
[0.279s][info   ][gc] GC(10) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.334ms
[0.281s][info   ][gc] GC(11) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.332ms
[0.282s][info   ][gc] GC(12) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.343ms
[0.286s][info   ][gc] GC(13) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.462ms
[0.288s][info   ][gc] GC(14) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.498ms
[0.290s][info   ][gc] GC(15) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.395ms
[0.292s][info   ][gc] GC(16) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.407ms
[0.294s][info   ][gc] GC(17) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.382ms
[0.296s][info   ][gc] GC(18) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.412ms
[0.298s][info   ][gc] GC(19) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.354ms
[0.300s][info   ][gc] GC(20) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.329ms
[0.302s][info   ][gc] GC(21) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.329ms
[0.304s][info   ][gc] GC(22) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.319ms
[0.305s][info   ][gc] GC(23) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.318ms
[0.307s][info   ][gc] GC(24) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.398ms
[0.309s][info   ][gc] GC(25) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.408ms
[0.311s][info   ][gc] GC(26) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.323ms
[0.313s][info   ][gc] GC(27) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.406ms
[0.315s][info   ][gc] GC(28) Pause Young (G1 Evacuation Pause) 7M->1M(10M) 0.420ms
Time cost is 64349578
```

速度提高到未优化的6倍.