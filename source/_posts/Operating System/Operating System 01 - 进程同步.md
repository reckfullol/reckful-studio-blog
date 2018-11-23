---
title: Operating System 01 - 进程同步
date: 2018-04-05 17:27:15
categories: Operating System
---
# 进程同步

<!--more-->

## 临界区

对临界资源访问的区域被称为**临界区**.

为了互斥访问临界资源, 每个进程在访问临界区之前, 需要进行检查.

```c
// entry section.
// critical section.
// exit section.
```

## 同步与互斥

- **同步**: 多个进程按一定顺序执行.
- **互斥**: 多个进程在同一时刻只有一个进程能进入临界区.

## 信号量

**信号量(Semaphore)** 是一个整形变量, 可以对其执行**down()** 和**up()** 操作, 也就是P和V操作.

- **down**: 如果信号量大于0, 执行-1操作; 如果信号量等于0, 进程睡眠, 等待信号量大于0.
- **up**: 对信号量执行+1操作, 唤醒睡眠的进程让其完成down操作.

down和up操作需要被设计成原语, 不可分割, 通常的做法是在执行这些操作的时候屏蔽中断.

如果信号量的取值只能是0或者1, 那么就称为了互斥量(Mutex), 0表示临界区已经加锁, 1表示临界区解锁.

```c
typedef int semaphore;
semaphore mutex = 1;
void P1() {
    down(&mutex);
    // 临界区
    up(&mutex);
}

void P2() {
    down(&mutex);
    // 临界区
    up(&mutex);
}
```

## 经典同步问题

### 消费者生产者问题

问题描述: 使用一个**缓冲区**来保存物品, 只有缓冲区没有满, 生产者才可以放入物品; 只有缓冲区不为空, 消费者才可以拿到物品.

1. 信号量

    因为缓冲区属于临界资源, 因此需要使用一个**互斥量mutex**来控制对缓冲区的互斥访问.

    为了同步生产者和消费者的行为, 需要记录缓冲区中物品的数量. 数量使用信号量来统计: **empty**记录空缓冲区的数量, **full**记录满缓冲区的数量. 因此empty信号量在生产者进程中使用, 当empty不为0时, 生产者才能放入物品; ful信号量在消费者进程中使用, 当full不为0时, 消费者才能拿走物品.

    这里要注意一点, **不能对缓冲区进行加锁后再测试信号量**. 也就是说, 不能在down(mutex)前执行down(empty/full). 因为当发现信号量(empty/full)为0时, 该进程会被挂起, 但临界区尚未被释放, 其余进程不能进入并进行up()操作, 导致生产者和消费者会一直等下去, 造成死锁.

    ```c
    const int N = 100;
    typedef int semaphore;

    semaphore mutex = 1;
    semaphore empty = N;
    semaphore full = 0;

    void Producer() {
        while(true) {
            int item = ProductItem();
            Down(&empty);
            Down(&mutex);
            InsertItem(item);
            Up(&mutex);
            Up(&full);
        }
    }

    void Consumer() {
        while(true) {
            Down(&full);
            Down(&mutex);
            int item = RemoveItem();
            Up(&mutex);
            Up(&empty);
            ConsumeItem(item);
        }
    }
    ```

2. 管程

    使用信号量机制实现的生产者消费者问题需要客户端代码做很多控制, 而管程把控制的代码独立出来, 不仅不容易出错, 也使得客户端代码调用更容易.

    ```pascal
    monitor ProducerConsumer
        integer i;
        condition c;

        procedure insert();
        begin
            // ...
        end;

        procedure remove();
        begin
            // ...
        end;
    end monitor;
    ```

    管程有一个重要特性: **在一个时刻只能有一个进程使用管程**. 进程在无法继续执行的时候不能一直占用管程, 否则其他进程永远不能使用管程.

    管程引入了条件变量以及相关的操作: **wait()** 和**singal()** 来实现同步操作. 对条件变量执行wait()操作会导致调用进程阻塞, 把管程让出来给另一个进程持有. singal()操作用于唤醒被阻塞的进程.

    ```pascal
    monitor ProducerConsumer
        condition full, empty;
        integer const := 0;
        condition c;

        procedure Insert(item: integer);
        begin
            if count = N then Wait(full);
            InsertItem(item);
            count := count + 1;
            if count = 1 the Singal(empty);
        end;

        function Remove: integer
        begin
            if count = 0 then Wait(empty);
            remove = RemoveItem;
            count := count - 1;
            if count = N - 1 then Singal(full);
        end;
    end monitor

    procedure Producer
    begin
        while true do
        begin
            item = ProduceItem;
            ProducerConsumer.Insert(item);
        end
    end;

    procedure Consumer
    begin
        while true do
        begin
            item = ProducerConsumer.Remove;
            ConsumeItem(item);
        end
    end;
    ```
### 读者写者问题

问题描述: 允许多个进程同时对数据进行读操作，但是不允许**读和写**以及**写和写**操作同时发生。

用一个整型变量**count**记录在对数据进行读操作的进程数量, 一个互斥量**countMutex**用于对count加锁, 一个互斥量**dataMutex**用于对读写的数据加锁.

```c
typedef int semaphore;

semaphore countMutex = 1;
semaphore dataMutex = 1;

int count = 0;

void Reader() {
    while(true) {
        Down(&countMutex);
        count++;
        if(count == 1) {
            Down(&dataMutex);
        }
        Up(&countMutex);
        Read();
        Down(&countMutex);
        count--;
        if(count == 0) {
            Up(&dataMutex);
        }
        Up(&countMutex);
    }
}

void Writer() {
    while(true) {
        Down(&dataMutex);
        Writer();
        Up(&dataMutex);
    }
}
```

### 哲学家问题

问题描述: 五个哲学家围着一张圆桌，每个哲学家面前放着食物。哲学家的生活有两种交替活动：吃饭以及思考。当一个哲学家吃饭时，需要先拿起自己左右两边的两根筷子，并且一次只能拿起一根筷子。

为了防止死锁的发生, 可以设置两个条件:

1. 必须同时拿起左右两根筷子.
2. 只有在两个邻居都没有进餐的情况下才允许进餐.

用互斥量**mutex**对餐桌加锁, 用信号量数组**s[N]** 修饰每个哲学家的状态.

```c
const int N = 5;
#define LEFT (i + N - 1)  % N
#define RIGHT (i + 1) % N

#define THINKING 0
#define HUNGRY 1
#define EATTING 2

typedef int semaphore;

int state[N];

semaphore mutex = 1;
semaphore s[N];

void Philosophere(int i) {
    while(true) {
        Think();
        TakeTwo(i);
        Eat();
        PutTwo(i);
    }
}

void TakeTwo(int i) {
    Down(&mutex);
    state[i] = HUNGRY;
    Test(i);
    Up(&mutex);
    Down(&s[i]);
}

void PutTwo(int i) {
    Down(&mutex);
    state[i] = THINKING;
    test(LEFT);
    test(RIGHT);
    Up(&mutex);
}

void Test(int i) {
    if(state[i] == HUNGRY && state[LEFT] != EATTING && state[RIGHT] != EATTING) {
        state[i] = EATTING;
        Up(&s[i]);
    }
}
```
