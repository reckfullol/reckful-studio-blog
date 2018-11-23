---
title: Operating System 04 - wait()
date: 2018-08-24 08:56:34
categories: Operating System
---
# wait()

<!--more-->

## 进程状态

|状态|说明|
|---|---|
|R|running or runnable(on run queue)|
|D|uninterruptible sleep(usually I/O)|
|S|inerruptible sleep(waiting for an event to complete)|
|Z|terminated but not reaped by it's parent|
|T|either by a job contro singnal or because it is being traced|

![process_status](https://res.cloudinary.com/dpe4i978o/image/upload/v1535072416/os/process_status.png)

## SIGCHLD

当一个子进程改变了他的状态时(停止运行, 继续运行或者退出), 会有两件事情发生在父进程.

1. 得到SIGCHLD信号
2. waitpid()或者wait()调动会返回

![SIGCHLD](https://res.cloudinary.com/dpe4i978o/image/upload/v1535072544/os/SIGCHLD.png)

其中子进程发送的SIGCHLD信号包含了子进程的信息, 包含了进程ID, 进程状态, 进程使用CPU时间等.

在子进程退出时, 他的进程描述符不会立即释放, 这是为了让父进程得到子进程信息, 父进程通过wait()或者waitpid()来获得一个已经退出的子进程信息.

### wait()

```c
pid_t wait(int* status)

父进程调用wait()会一直阻塞, 直到收到一个子进程退出的SIGCHLD信号, 之后wait()函数会销毁子进程并返回.

如果成功, 返回被收集的子进程的进程ID, 如果调用进程没有子进程, 调用会失败, 此时返回-1, 同时errno被置为ECHILD.

参数status用来保存被收集的子进程退出时的一些状态, 如果对这个子进程是如何死掉的毫不在意, 只想把这个子进程消灭掉, 可以设置这个参数为NULL.
```

## waitpid()

```c
pid_t waitpid(pid_t pid, int* status, int options)
```

作用和wait()相同, 但是多了两个可由用户控制的参数pid和options.

pid参数指示这个子进程的ID, 表示只关心这个子进程退出的SIGCHLD信号, 如果结果为-1, 那么和wait()作用相同, 都是关心所有子进程退出的SIGCHLD信号.

options参数主要有WNOHANG和WUNTRACED两个, 前者可以使waitpid(), 调用变成非阻塞的, 也就是说他会立即返回, 父进程可以继续执行其他任务.