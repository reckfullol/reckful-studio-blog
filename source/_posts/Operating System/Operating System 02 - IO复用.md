---
title: Operating System 02 - IO复用
date: 2018-04-09 16:33:18
categories: Operating System
---
# IO复用

<!--more-->

## 概念

I/O Multiplexing 又被称为 Event Driven I/O, 它可以让单个进程具有处理多个 I/O 事件的能力.

当某个 I/O 事件条件满足时, 进程会收到通知.

如果一个 Web 服务器没有 I/O 复用, 那么每一个 socket 连接都需要创建一个线程去连接. 如果同时连接几万个连接, 那么就需要创建相同数量的线程, 并且相比于多进程和多线程技术, I/O 复用不需要进程线程创建和切换的开销, 系统的开销更小.

## I/O 模型

- 阻塞(Blocking)
- 非阻塞(Non-blocking)
- 同步(Synchronous)
- 异步(Asynchronous)

阻塞非阻塞是等待 I/O 完成的方式, 阻塞要求用户程序停止执行, 直到 I/O 完成, 而非阻塞在 I/O 完成之前还可以继续执行.

同步异步是获知 I/O 完成的方式, 同步需要时刻关心 I/O 是否已经完成, 异步无需主动关心, 在 I/O 完成时它会收到通知.

![io_modeel](https://res.cloudinary.com/dpe4i978o/image/upload/v1523264238/IO/54cb3f21-485b-4159-8bf5-dcde1c4d4c36.png)

### 同步-阻塞

这是最常见的一种模型, 用户程序在使用read()时会执行系统调用从而陷入内核, 之后就被阻塞直到系统调用完成.

应该注意到, 在阻塞的过程中, 其他程序还可以执行, 因此阻塞不意味着整个操作系统都被阻塞, 因为其他程序还可以执行, 因此不消耗CPU时间, 这种模型的执行效率会比较高.

![sync_block](https://res.cloudinary.com/dpe4i978o/image/upload/v1523264158/IO/5e9b10f3-9504-4483-9667-d4770adebf9f.png)

### 同步-非阻塞

非阻塞意味着用户程序在执行系统调用后还可以继续执行, 内核并不是马上执行完 I/O, 而是以一个错误码来告知用户程序 I/O 还未完成. 为了获得 I/O 完成时间, 用户程序必须调用多次系统调用去询问内核, 甚至是忙等, 也就是在一个循环里面一直询问并等待.

由于 CPU 要处理更多的用户程序的询问, 因此这种模型的效率是比较低的.

![sync-nonblock](https://res.cloudinary.com/dpe4i978o/image/upload/v1523264176/IO/1582217a-ed46-4cac-811e-90d13a65163b.png)

### 异步-阻塞

这是 I/O 复用使用的一种模式, 通过使用 select, 它可以监听多个 I/O 事件, 当这些事件至少有一个发生时, 用户程序会收到通知.

![async-block](https://res.cloudinary.com/dpe4i978o/image/upload/v1523264196/IO/dbc5c9f1-c13c-4d06-86ba-7cc949eb4c8f.jpg)

### 异步-非阻塞

在该模式下, I/O 操作会立刻返回, 之后可以处理其他操作, 并且在 I/O 完成时会收到一个通知, 此时会中断正在处理的操作, 然后继续之前的操作.

![async-nonblock](https://res.cloudinary.com/dpe4i978o/image/upload/v1523264176/IO/1582217a-ed46-4cac-811e-90d13a65163b.png)

## select poll epoll

这三个都是 I/O 多路复用的具体实现, select 出现的最早, 之后是 poll, 再是epoll.

### select

```c
int select(int n, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
```

- fd_set 表示描述符集合
- readset, writeset, exceptset 这三个参数指定让操作系统内核测试读, 写和一场条件的描述符
- timeout 参数告知内核等待所指定描述符中的任何一个就绪可花多少时间
- 成功调用返回结果大于0, 出错返回结果为-1, 超时返回结果为0

```c
fd_set fd_in, fd_out;
struct timeval tv;

// reset sets
FD_ZERO(&fd_in);
FD_ZERO(&fd_out);

// monitor sock1 for input events
FD_SET(sock1, &fd_in);

// monitor sock2 for output events
FD_SET(sock2, &fd_out);

// find out which socket has the largest numeric value as select requires it
int largest_sock = sock1 > sock2 ? sock1 : sock2;

// wait up to 10 seconds
tv.tv_sec = 10;
tv.tv_usec = 0;

// call the select
int ret = select(largest_sock + 1, &fd_in, &fd_out, NULL, &tv);

// check if select actually succeed
if(ret == -1) {
    // report error and abort
} else if(ret == 0) {
    // timeout, no event detected
} else {
    if(FD_ISSET(sock1, &fd_in)) {
        // input event on sock1
    }
    if(FD_ISSET(sock2, &fd_out)) {
        // output event on sock2
    }
}
```

每次调用 select() 都需要将 fd_set* readfds, writefds, exceptfds 内容全部从用户进程内存中复制到操作系统内核中, 内核需要将所有 fd_set 遍历一遍, 这个过程非常低效.

返回结果中内核并没有声明哪些 fd_set 已经准备好了, 所以如果返回值大于0时, 程序需要遍历所有的 fd_set 判断哪个 I/O 已经准备被好了.

在 Linux 中 select 最多支持 1024个 fd_set 同时轮询, 其中 1024 由 Linux 内核的 FD_SETSIZE 决定. 如果需要打破该限制可以修改 FD_SETSIZE, 然后重新编译内核.

### poll

```c
int poll(struct pollfd *fds, unsigned int nfds, int timeout);
```

```c
struct pollfd {
    int fd;
    // 监视的请求事件
    short events;
    // 已发生的事件
    short revents;
}

struct poolfd fds[2];

// monitor sock1 for input
fds[0].fd = sock1;
fds[0].events = POLLIN;

// monitor sock2 for output
fds[1].fd = sock2;
fds[1].events = POLLOUT;

// wait 10 seconds
int res = poll(&fds, 2, 10000);

// check if poll actually succed
if(res == -1) {
    // report error and abort
} else if (res == 0) {
    // timeout, no event detected
} else {
    // if we detect the event, zero it out so we can reuse the strutcure
    if(fds[0].revents & POLLIN) {
        fds[0].revents = 0;
        // input event on sock1
    }
    if(fds[1].revents & POLLOUT) {
        fds[1].revents = 0;
        // output event on sock2
    }
}
```

它和 select() 功能基本相同, 同样需要每次将 fds 复制到内核, 返回后同样需要进行轮询每一个 pollfd 是否已经 I/O 准备好. poll() 取消了1024个描述符数量上限, 但是数量太大以后不能保证执行效率, 因此复制大量内存到内核十分低效, 所需实际时间与描述符数量成正比. poll() 在 pollfd 的重复利用上比 select() 的 fd_set 会更好.

如果在多线程下, 如果一个线程对某个描述符调用了 poll() 系统调用, 但是另一个线程关闭了该描述符, 会导致 poll() 调用结果不确定, 该问题同样出现在 select() 中.

### epoll

```c
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```

```c
// create the epoll descriptor, only one is needed per app, and is used to monitor all sockets
// the fuction argument is ignored(it was not before, but now it is), so put your favorite number here
int pollingfd = epoll_create(0xCAFE);

if(pollingfd < 0) {
    // report error
} 

// initialize the epoll structure in case more members are added in future
struct epoll_event ev = {0};

// associate the connection class instance with the event, you can associate anything
// you want, epoll dose not use the information. we store a connection class pointer, pConnection1
ev.data.ptr = pConnection1

// monitor for input, and do not automatically rearm the descriptor after the event
ev.events = EPOLLIN | EPOLLONESHOT;

// add the descriptor into the monitoring list, we can do it even if another thread is
// waiting in epoll_wait - the descriptor will be properly added
if(epoll_ctl(epollfd, EPOLL_CTL_ADD, pConnection1->getSocket(), &ev) != 0) {
    // report error
}

// wait for up to 20 events(assuming we have added maybe 200 sockets before that it may happen)
struct epoll_event prevents[20];

// wait for up to 10 secounds, and retrieve less than 20 epoll_event and store them into epoll_event array
int ret = epoll_wait(pollingfd, prevents, 20, 10000);

// check if epoll actually succeed
if(ret == -1) {
    // report error and abort
} else if(ret == 0) {
    // timeout, no event detected
} else {
    for(int i = 0; i < ret; i++) {
        if(prevents[i].events & EPOLLIN) {
            // get back our connection pointer
            Connection* c = (Connection*) prevents[i].data.ptr;
            c->handleReadEvent();
        }
    }
}
```

epoll 仅仅适用于 Linux.

它是 select 和 poll 的增强版, 更加灵活而且没有描述符限制. 它将用户关心的描述符放到内核的一个事件表中, 从而只需要用户空间和内核空间拷贝一次.

select 和 poll 方式中, 进程只有在调用一定的方法后, 内核才对所有监视的描述符进行扫描. 而 epoll 事先通过 epoll_ctl() 来注册描述符, 一旦基于某个描述符就绪时, 内核会采用类似 callback 的回调机制, 迅速激活这个描述符, 当进程调用epoll_wait() 时便得到通知.

新版的 epoll_create(int size) 参数 size 不起任何作用, 在老版本中如果描述符的数量大于 size, 不保证服务质量.

epoll_ctl() 执行一次系统调用, 用于向内核注册新的描述符或者是改变某个文件描述符的状态. 已注册的描述符在内核中会被维护在一颗红黑树上, 通过回调函数内核会将 I/O 准备好的描述符加入到一个链表中管理.

epoll_wait() 取出在内核中通过链表维护的 I/O 准备好的描述符, 将他们从内核复制到程序中, 不需要像 select / poll 对注册的所有描述符遍历一遍.

epoll 对多线程编程更友好, 同时多个线程对同一个描述符调用了 epoll_wait 也不会产生像 select / poll 的不确定情况. 或者一个线程调用了 epoll_wait 另一个线程关闭了同一个描述符也不会产生不确定情况.

#### epoll 工作模式

epoll_event 有两种触发模式: LT(level trigger) 和 ET(edge trigger)

1. LT 模式

    当 epoll_wait() 检测到描述符事件发生并将此事件通知应用程序, 应用程序可以不立即处理该事件. 下次调用 epoll_wait() 时, 会再次响应应用程序并通知事件. 是默认的一种模式, 并且同时支持 blocking 和 non-blocking.

2. ET 模式

    当 epoll_wait() 检测到描述符事件发生并将此事件通知应用程序, 应用程序必须立即处理该事件. 如果不处理, 下次调用 epoll_wait() 时, 不会再次响应应用程序并通知此事件. 很大程度上减少了 epoll 事件被重复触发的次数, 因此效率要比 LT 模式高. 只支持 non-blokcing, 以避免由于一个文件句柄的阻塞读/写操作把处理多个文件描述符的任务饿死.

#### EPOLLONESHOT

即使我们使用了ET模式, 一个socket上的某个事件还是可能被触发多次. 这在并发中就可能引发, 比如一个线程在读取完某个socket上的数据后再开始处理这些数据, 而在数据的处理过程中该socket上又有新数据可读, EPOLLIN再次被触发, 此时另外一个线程被唤醒来读取这些新的数据. 于是就会出现两个线程同时操作一个socket的局面. 而我们期望的是一个socket连接在任意时刻都只被一个线程处理, 因此我们可以使用epoll的EPOLLONESHOT事件来实现.

对于注册了EPOLLONESHOT事件的文件描述符, 操作系统最多触发其注册的一个可读, 可写或者异常事件, 而且只出发一次, 除非我们使用epoll_ctl函数重制该文件描述符上注册的EPOLLONESHOT事件.