---
title: Go 01 - Goroutine
date: 2019-09-17 09:16:28
categories: Go
---
# Goroutine

<!--more-->

## 并发与并行

### 并发

并发是指程序的逻辑结构. 非并发程序只有一个逻辑控制流的顺序执行程序, 在任何时刻, 程序只会处于在这个逻辑流的某个位置. 如果一个程序有多个独立的逻辑控制流, 那么就说这个程序是并发的. 也就是说, 如果把逻辑控制流绘制为时序流程图, 那么允许出现重叠的区间.

并发可以通过以下方式做到:

1. 显式定义并触发多个逻辑控制流, 由应用程序或操作系统对其进行调度. 他们之间可以是独立无关, 也可以是相互依赖.
2. 隐式放置多个逻辑控制流, 由事件触发时调用, 也就是事件驱动.

### 并行

并行是指程序的运行状态. 如果一个程序在某一时刻可以被多个CPU流水线同时处理, 那么我们就说这个程序是以并行的方式运行. 显然并行是需要硬件支持.

并行可以通过以下方式做到:

1. 多台物理机器, 如cluster进行MapReduce.
2. 多CPU流水线, 多个CPU或者多核或者超线程.
3. 单CPU中的Instruction-level parallelism, 指令级并行. 通过分支预测和乱序执行, 可以令CPU在单个时钟周期内执行多条指令.
4. Single instruction, multiple data, SIMD指令集支持单条指令对多条数据进行操作.

### 关系

1. 并发是并行的必要不充分条件, 如果一个程序本身就不是并发的, 那么不可能被并行化处理. 一个并发程序如果只能被一个CPU流水线用分时方式处理, 那么就不是并行的.
2. 并发只是更符合现实问题本质的表达方法, 并发的最初目的是简化代码逻辑, 而不是使程序运行的更快.

![](https://i.loli.net/2019/10/07/9gfXEUk7oRqlOZy.png)

## 线程模型

![](https://i.loli.net/2019/10/08/ujNvGbRpEIMrnUk.png)

一般意义上的线程模型根据用户态和内核态之间的对应关系一般分为三类:

1. N:1 :
   
   所有用户态线程对应到一个内核态线程, 这样上下文切换成本最低, 但是无法利用多核资源.

2. 1:1 :
   
    一个用户态线程对应到一个内核态线程, 能够利用多核资源, 但是上下文切换成本较高.

3. M:N :

    若干个用户态线程绑定到不同到内核态线程, 既能够利用多核资源, 也能够降低上下文切换时间, 但是调度算法但实现成本偏高.

## 为什么需要Go Scheduler

![](https://i.loli.net/2019/10/08/HPV6QBma9okzr34.png)

1. 避免线程创建开销: 对于内核态线程而言, 很多特性是操作系统给予的. 但对于Go程序很多是不必要的, 如果每次创建协程都创建一个内核态线程与之对应, 为了没有必要的特性而牺牲性能是不经济的.
2. 减少Go垃圾回收的复杂度: 根据Go的垃圾回收器机制, 需要在垃圾回收时暂停所有线程, 才能使得内存到达稳定一致的状态, 对于1:1的方案令所有内核态线程暂停开销过大.

## Go Scheduler

![](https://i.loli.net/2019/10/08/8cPnryGCqYBdQl7.png)

Go Scheduler是参考CSP(Communicating Sequential Process)模型, 使用Goroutine并行执行任务, 并将Channel作为Goroutine之间的通信方式. 虽然使用互斥锁和共享内存也可以在Go中完成Goroutine之间的通信, 但是使用Channel更为推荐.

> Don't communicate by sharing memory, share memory by communicating.

Go Scheduler采用M:N两级线程模型, 即GMP模型.

1. M: 表示操作系统中的线程, 他是可以被操作系统管理的线程, 和POSIX中的标准线程非常类似.
2. G: 表示Goroutine, 每个Goroutine都包含堆栈, 指令指针和其他用于调度的信息.
3. P: 表示调度的上下文, 可以被看作一个运行线程M上的本地调度器.

### G

Go语言中并发的执行单元就是Goroutine, 类似于操作系统中的线程, 但是占用了更小的内存空间, 同时降低了Goroutine切换的开销.


```go
type g struct {
    m              *m               // current m; offset known to arm liblink
    sched          gobuf
    syscallsp      uintptr          // if status==Gsyscall, syscallsp = sched.sp to use during gc
    syscallpc      uintptr          // if status==Gsyscall, syscallpc = sched.pc to use during gc
    param          unsafe.Pointer   // passed parameter on wakeup
    atomicstatus   uint32
    goid           int64
    schedlink      guintptr
    waitsince      int64            // approx time when the g become blocked
    waitreason     waitReason       // if status==Gwaiting
    preempt        bool             // preemption signal, duplicates stackguard0 = stackpreempt
    lockedm        muintptr
    writebuf       []byte
    sigcode0       uintptr
    sigcode1       uintptr
    sigpc          uintptr
    gopc           uintptr          // pc of go statement that created this goroutine
    startpc        uintptr          // pc of goroutine function
    waiting        *sudog           // sudog structures this g is waiting on (that have a valid elem ptr); in lock order
}
```

结构体g的字段atomicsatatus就存储了Goroutine的状态:

|||
|---|---|
|_Gidle|	刚刚被分配并且还没有被初始化|
|_Grunnable|	没有执行代码, 没有栈的所有权, 存储在运行队列中|
|_Grunning|	可以执行代码, 拥有栈的所有权, 被赋予了内核线程 M 和处理器 P|
|_Gsyscall|	正在执行系统调用, 拥有栈的所有权, 没有执行用户代码, 被赋予了内核线程 M 但是不在运行队列上|
|_Gwaiting|	由于运行时而被阻塞, 没有执行用户代码并且不在运行队列上, 但是可能存在于 Channel 的等待队列上|
|_Gdead|	没有被使用, 没有执行代码, 可能有分配的栈|
|_Gcopystack|	栈正在被拷贝, 没有执行代码, 不在运行队列上|

最终我们可以将Goroutine的运行期总结为三种状态:
1. 等待中: 表示当前Goroutine等待某些条件满足后才会继续执行, 比如当前Goroutine正在执行系统调用或者同步操作.
2. 可运行: 表示当前Goroutine在等待某个M执行其指令, 如果当前程序中有非常多都Goroutine, 每个Goroutine就可能会等待更多都时间.
3. 运行中: 表示当前Goroutine正在某个M上执行指令.

### M

Go并发模型中M其实表示的是操作系统线程, 默认情况下调度器最多能创建10000个线程, 但是其中绝大多数线程都不会执行用户代码, 最多只有GOMAXPROCS个线程M能够正常运行.

在默认情况下, 一个四核机器会创建四个操作系统线程, 每个线程其实都是一个m结构体, 我们也可以通过runtime.GOMAXPROCS改变最大可运行线程的个数.

在大多数情况下, 我们都会使用Go的默认配置, 也就是#thread = #CPU, 在这种情况下不会触发操作系统级别的线程调度和上下文切换, 所有的调度都会发生在用户态, 由Go语言调度器触发, 能够减少非常多的额外开销.

```go
type m struct {
    g0      *g     // goroutine with scheduling stack
    curg    *g     // current running goroutine

    ...
}
```

g0是持有调度堆栈的Goroutine, curg是当前线程上运行的Goroutine, 这也是作为操作系统唯二关心的Goroutine了.

### P

P是线程需要的上下文环境, 也是用于处理代码逻辑的处理器, 通过处理器的调度, 每个内核线程M都能够执行多个G, 这样就能在G进行一些IO操作的时候及时对他们进行切换, 提高CPU的利用率.

每个Go程序中之所以处理器的数量一定会等于GOMAXPROCS, 是因为调度器在启动时就会创建GOMAXPROCS个处理器P, 这些处理器会绑定到不同的线程M上并为他们调度Goroutine.

```go
type p struct {
    id          int32
    status      uint32     // one of pidle/prunning/...
    link        puintptr
    schedtick   uint32     // incremented on every scheduler call
    syscalltick uint32     // incremented on every system call
    sysmontick  sysmontick // last tick observed by sysmon
    m           muintptr   // back-link to associated m (nil if idle)
    mcache      *mcache

    runqhead uint32
    runqtail uint32
    runq     [256]guintptr
    runnext guintptr

    sudogcache []*sudog
    sudogbuf   [128]*sudog

    ...v
}
```

p结构体中的状态status会是以下五种状态之一:

|||
|---|---|
|_Pidle|	处理器没有运行用户代码或者调度器, 被空闲队列或者改变其状态的结构持有, 运行队列为空|
|_Prunning|	被线程 M 持有, 并且正在执行用户代码或者调度器|
|_Psyscall|	没有执行用户代码, 当前线程陷入系统调用|
|_Pgcstop|	被线程 M 持有, 当前处理器由于垃圾回收被停止|
|_Pdead|	当前处理器已经不被使用|

### 调度

![](https://i.loli.net/2019/10/08/6Kba4hOecZ9JAHq.png)

如图所示, 我们现在有两个线程(M), 每个都持有一个上下文环境(P), 同时运行着一个协程(G). 为了执行若干协程指令, 每个线程必须持有一个上下文环境.

灰色的协程表示状态为等待中, 即没有执行代码, 没有栈的所有权, 等待在运行队列中. 当Go程序中执行到go表达式时, 协程会被添加到运行队列的队尾. 当上下文环境到达调度点, 会从运行队列队首弹出一个协程, 为其设置堆栈, 指令指针, 开始执行其指令.

为了减少加锁竞态, 每个上下文环境都有一个自己的运行队列. 在早期, Go scheduler共享一个全局运行队列. 线程需要频繁加锁去获取等待执行的协程. 因此当拥有多核的机器时, 当时的goroutine的性能并不理想.

#### 系统调用

![](https://i.loli.net/2019/10/08/tV1oRPTLzjm38Hk.png)

为什么我们不舍弃上下文(P), 将运行队列直接挂载到线程. 原因在于当执行线程囿于一些原因阻塞时, 我们可以通过保有上下文环境携带运行队列挂载到其他线程下.

一种阻塞的原因是当我们调用系统调用, 因为线程无法同时既执行指令又被系统调用阻塞, 我们需要卸载上下文环境, 令其可以继续保持调度状态.

Go scheduler保证环境中存在足够的线程运行所有的上下文环境. 以上图举例, M1是一个或新创建或从线程池取出的一个线程. 这个系统调用的线程M0会继续持有协程G0完成系统调用.

当系统调用结束, 线程M0会尝试获得上下文环境来运行协程. 通常他会从其他线程中窃取一个上下文环境, 如果窃取失败, 线程会将这个协程G0放到全局的运行队列, 并把自己放回线程池, 切换到休眠状态.

上下文环境发现本地运行队列为空时, 会从这个全局队列中拉取协程. 上下文环境同时也会周期性的检查全局运行队列, 避免有协程出现饿死的情况.

需要注意的是, 如果协程调用网络I/O, 会从线程中脱离出来并放入运行时集成的网络poller中, 当poller表示网络的读写操作完成后, 这个协程会回到上下文环境并完成后续工作.

#### 窃取工作

![](https://i.loli.net/2019/10/08/9IpLPGicUqla14s.png)

某个上下文环境执行完了本地运行队列的所有协程后, 会导致若干本地运行队列的不平衡. 因此该上下文环境会首先从全局运行队列中获取协程, 如果没有则会尝试从其他上下文环境中拿走一半的协程. 


## 性能测试

### 耗时

![](https://i.loli.net/2019/10/10/Y6wKgISUPfcTMvb.png)

横轴: 协程数量级.
纵轴: 耗时(ns/op).
红线: 创建单个协程所需要的时间.
绿色: 切换单个协程所需要的时间.

由上图可以看出, 在1e3到1e5的规模下, 协程的创建和切换时间开销都较为稳定:

创建单个协程: ~1500ns/op.
切换单个协程: ~500ns/op.

### 内存

![](https://i.loli.net/2019/10/10/deElJitANacz87S.png)

由上图可以看出, 在1e5到1e6到区间, 单个线程的内存占用都在2500B左右.

### 测试代码

```go
// memory usage
package main

import (
    "flag"
    "fmt"
    "os"
    "runtime"
    "time"
)

var n = flag.Int("n", 1e5, "Number of goroutines to create")

var ch = make(chan byte)
var counter = 0

func f() {
    counter++
    <-ch // Block this goroutine
}

func main() {
    flag.Parse()
    if *n <= 0 {
        fmt.Fprintf(os.Stderr, "invalid number of goroutines")
        os.Exit(1)
    }

    // Limit the number of spare OS threads to just 1
    runtime.GOMAXPROCS(1)

    // Make a copy of MemStats
    var m0 runtime.MemStats
    runtime.ReadMemStats(&m0)

    t0 := time.Now().UnixNano()
    for i := 0; i < *n; i++ {
        go f()
    }
    runtime.Gosched()
    t1 := time.Now().UnixNano()
    runtime.GC()

    // Make a copy of MemStats
    var m1 runtime.MemStats
    runtime.ReadMemStats(&m1)

    if counter != *n {
        fmt.Fprintf(os.Stderr, "failed to begin execution of all goroutines")
        os.Exit(1)
    }

    fmt.Printf("Number of goroutines: %d\n", *n)
    fmt.Printf("Per goroutine:\n")
    fmt.Printf("  Memory: %.2f bytes\n", float64(m1.Sys-m0.Sys)/float64(*n))
    fmt.Printf("  Time:   %f µs\n", float64(t1-t0)/float64(*n)/1e3)
}
```

```go
// time consume
package main

import (
    "fmt"
    "time"
)

func runCallback(in, out chan int64) {
    for n, ok := <-in; ok; n, ok = <-in {
        out <- n
    }
}

func runTest(round int, coroutineNum, switchTimes int64) {
    fmt.Printf("##### Round: %v\n", round)

    start := time.Now()
    channelsIn, channelsOut := make([]chan int64, coroutineNum), make([]chan int64, coroutineNum)
    for i := int64(0); i < coroutineNum; i++ {
        channelsIn[i] = make(chan int64, 1)
        channelsOut[i] = make(chan int64, 1)
    }
    end := time.Now()
    fmt.Printf("Create %v goroutines and channels cost %v ns, avg %v ns\n", coroutineNum, end.Sub(start).Nanoseconds(), end.Sub(start).Nanoseconds()/coroutineNum)

    start = time.Now()
    for i := int64(0); i < coroutineNum; i++ {
        go runCallback(channelsIn[i], channelsOut[i])
    }
    end = time.Now()
    fmt.Printf("Start %v goroutines and channels cost %v ns, avg %v ns\n", coroutineNum, end.Sub(start).Nanoseconds(), end.Sub(start).Nanoseconds()/coroutineNum)

    var sum int64 = 0
    start = time.Now()
    for i := int64(0); i < switchTimes; i++ {
        for j := int64(0); j < coroutineNum; j++ {
            channelsIn[j] <- 1
            sum += <-channelsOut[j]
        }
    }
    end = time.Now()
    fmt.Printf("Switch %v goroutines for %v times cost %v ns, avg %v ns\n", coroutineNum, sum, end.Sub(start).Nanoseconds(), end.Sub(start).Nanoseconds()/sum)

    start = time.Now()
    for i := int64(0); i < coroutineNum; i++ {
        close(channelsIn[i])
        close(channelsOut[i])
    }
    end = time.Now()
    fmt.Printf("Close %v goroutines cost %v ns, avg %v ns\n", coroutineNum, end.Sub(start).Nanoseconds(), end.Sub(start).Nanoseconds()/coroutineNum)
}

func main() {
    // runTest(1, 1000, 1000)
    // runTest(2, 5000, 200)
    // runTest(3, 10000, 100)
    // runTest(4, 50000, 50)
    // runTest(5, 100000, 10)
    runTest(6, 500000, 5)
}
```

## Reference

- [Go scheduler](http://morsmachine.dk/go-scheduler)
- [Go in action](http://sufuq.com/books/golang/Go%20in%20Action.pdf)
- [Draveness' blog](https://draveness.me/golang/concurrency/golang-goroutine.html)
- [OWenT's gist](https://gist.github.com/owt5008137)