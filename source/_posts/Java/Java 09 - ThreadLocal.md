---
title: Java 09 - ThreadLocal
date: 2018-07-24 03:59:19
categories: Java
---
# ThreadLocal

<!--more-->

```java
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

class ThreadSpecificLog {
    private PrintWriter printWriter = null;

    ThreadSpecificLog(String filename) {
        try {
            printWriter = new PrintWriter(new FileWriter(filename));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    void println(String s) {
        printWriter.println(s);
    }

    void close() {
        printWriter.println("end of log");
        printWriter.close();
    }
}

class Log {
    private static final ThreadLocal<ThreadSpecificLog> threadSpecificLogCollection = new ThreadLocal<>();

    private static ThreadSpecificLog getThreadSpecificLog() {
        ThreadSpecificLog threadSpecificLog = threadSpecificLogCollection.get();

        if(threadSpecificLog == null) {
            threadSpecificLog = new ThreadSpecificLog(Thread.currentThread().getName() + "-log.txt");
            threadSpecificLogCollection.set(threadSpecificLog);
        }
        return threadSpecificLog;
    }

    static void println(String s) {
        getThreadSpecificLog().println(s);
    }

    static void close() {
        getThreadSpecificLog().close();
    }
}

class ClientThread extends Thread {
    ClientThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        System.out.println(getName() + " BEGIN");

        for(int i = 0; i < 10; i++) {
            Log.println("i = " + i);
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        Log.close();
        System.out.println(getName() + " END");
    }
}

public class Test {
    public static void main(String[] args) {
        new ClientThread("Alice").start();
        new ClientThread("Bobby").start();
        new ClientThread("Chris").start();
    }
}
```