---
title: Java 08 - 线程池
date: 2018-07-22 15:49:58
categories: Java
---
# 线程池

<!--more-->

```java
import java.util.Random;

class WorkerThread extends Thread {
    private final Channel channel;

    WorkerThread(String name, Channel channel) {
        super(name);
        this.channel = channel;
    }

    @Override
    public void run() {
        while(true) {
            Request request = channel.takeRequest();
            request.execute();
        }
    }
}

class Channel {
    private static final int MAX_REQUEST = 100;
    private final Request[] requestQueue;
    private int tail;
    private int head;
    private int count;

    private final WorkerThread[] threadPool;

    Channel(int threads) {
        this.requestQueue = new Request[MAX_REQUEST];
        this.head = 0;
        this.tail = 0;
        this.count = 0;

        threadPool = new WorkerThread[threads];

        for(int i = 0; i < threadPool.length; i++) {
            threadPool[i] = new WorkerThread("Worker-" + i, this);
        }
    }

    void startWorkers() {
        for(int i = 0; i < threadPool.length; i++) {
            threadPool[i].start();
        }
    }

    synchronized void putRequest(Request request) {
        while(count >= requestQueue.length) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        requestQueue[tail] = request;
        tail = (tail + 1) % requestQueue.length;
        count++;
        notifyAll();
    }

    synchronized Request takeRequest() {
        while(count <= 0) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        Request request = requestQueue[head];
        head = (head + 1) % requestQueue.length;
        count--;
        notifyAll();
        return request;
    }
}

class Request {
    private final String name;
    private final int number;
    private static final Random random = new Random();

    Request(String name, int number) {
        this.name = name;
        this.number = number;
    }

    void execute() {
        System.out.println(Thread.currentThread().getName() + " executes " + this);
        try {
            Thread.sleep(random.nextInt(1000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public String toString() {
        return "{ Request from " + name + " No." + number + " }";
    }
}

class ClientThread extends Thread {
    private final Channel channel;
    private static final Random random = new Random();

    ClientThread(String name, Channel channel) {
        super(name);
        this.channel = channel;
    }

    @Override
    public void run() {
        try {
            for(int i = 0; true; i++) {
                Request request = new Request(getName(), i);
                channel.putRequest(request);
                Thread.sleep(random.nextInt(1000));
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class Test {
    public static void main(String[] args) {
        Channel channel = new Channel(5);
        channel.startWorkers();
        new ClientThread("Alice", channel).start();
        new ClientThread("Bobby", channel).start();
        new ClientThread("Chris", channel).start();
    }
}
```