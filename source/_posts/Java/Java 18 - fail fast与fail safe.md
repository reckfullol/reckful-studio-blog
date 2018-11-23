---
title: Java 18 - fail fast与fail safe
date: 2018-07-31 00:16:32
categories: Java
---
# fail fast与fail safe

<!--more-->

## 定义

1. fail fast

    在用迭代器遍历集合对象的时候, 如果遍历过程对集合对象的内容进行了修改(添加, 删除), 那么会抛出ConcurrentModificationException.

    因为迭代器在遍历时直接访问集合中的内容, 并且在遍历过程中使用modCount, 集合在遍历期间如果内容发生变化了, 那么就会改变modCount值. 每当迭代器使用hashNext()/next()遍历下一个元素的时候, 都会检测modCount变量是否为expectedModCount, 如果是正常返回, 否则抛出异常.

    抛出异常的条件是检测到modCount != expectedModCount, 如果发生变化时修改了modCount同时有设置了expectedModCount, 那么异常不会抛出. 所以不能依赖异常的抛出来进行并发操作.

    我们以ArrayList里的remove函数为例:

    ```java
    public E remove(int index) {
        RangeCheck(index);

        modCount++;
        E oldValue = (E) elementData[index];

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index, numMoved);
        elementData[--size] = null;

        return oldValue;
    }
    ```

    无论是add(), remove()还是clear(), 只要是修改了集合中的元素个数, 都会改变modCount, 再接下来的遍历中, 就会导致异常的抛出.

2. fail safe
   
    我们以CopyOnWriteArrayList为例. 这种容器实现了读写的分离, 读操作是在原始数组中进行, 写操作是在一个复制的数组上进行. 写操作需要枷锁, 防止并发写入导致数据丢失. 写结束后将原数组指向到新的数组.

    ```java
    public boolean add(E e) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            Object[] elements = getArray();
            int len = elements.length;
            Object[] newElements = Arrays.copyOf(elements, len + 1);
            newElements[len] = e;
            setArray(newElements);
            return true;
        } finally {
            lock.unlock();
        }
    }

    final void setArray(Object[] a) {
        array = a;
    }
    @SuppressWarnings("unchecked")
    private E get(Object[] a, int index) {
        return (E) a[index];
    }
    ```

    CopyOnWriteArrayList适用于读多写少的场景, 但是缺点在于每次复制数组的开销的数据的不一致. 因此不适用于对内存敏感和实时性要求高的场景.