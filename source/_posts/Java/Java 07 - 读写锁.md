---
title: Java 07 - 读写锁
date: 2018-07-21 19:20:15
categories: Java
---
# 读写锁

<!--more-->

```java
import java.util.Random;

class Data {
    private final char[] buffer;
    private final ReadWriteLock lock = new ReadWriteLock();
    Data(int size) {
        this.buffer = new char[size];
        for(int i = 0; i < size; i++) {
            buffer[i] = '*';
        }
    }

    char[] read() throws InterruptedException {
        lock.readLock();
        try {
            return doRead();
        } finally {
            lock.readUnlock();
        }
    }

    void write(char c) throws InterruptedException {
        lock.writeLock();
        try {
            doWrite(c);
        } finally {
            lock.writeUnlock();
        }
    }

    private char[] doRead() {
        char[] newBuffer = new char[buffer.length];
        System.arraycopy(buffer, 0, newBuffer, 0, newBuffer.length);
        slowly();
        return newBuffer;
    }

    private void doWrite(char c) {
        for(int i = 0; i < buffer.length; i++) {
            buffer[i] = c;
            slowly();
        }
    }

    private void slowly() {
        try {
            Thread.sleep(50);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}

class WriterThread extends Thread {
    private static final Random random = new Random();
    private final Data data;
    private final String filler;
    private int index = 0;

    WriterThread(Data data, String filler) {
        this.data = data;
        this.filler = filler;
    }

    @Override
    public void run() {
        try {
            while(true) {
                char c = nextChar();
                data.write(c);
                Thread.sleep(random.nextInt(3000));
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private char nextChar() {
        if(index >= filler.length()) {
            index = 0;
        }
        char c = filler.charAt(index);
        index++;
        return c;
    }
}

class ReaderThread extends Thread {
    private final Data data;

    ReaderThread(Data data) {
        this.data = data;
    }

    @Override
    public void run() {
        try {
            while(true) {
                char[] readBuffer = data.read();
                System.out.println(Thread.currentThread().getName() + " read " + String.valueOf(readBuffer));
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class ReadWriteLock {
    private int readingReaders = 0;
    private int waitingWriters = 0;
    private int writingWriters = 0;
    private boolean preferWriter = true;

    synchronized void readLock() throws InterruptedException {
        while(writingWriters > 0 || (preferWriter && waitingWriters > 0)) {
            wait();
        }
        readingReaders++;
    }

    synchronized void readUnlock() {
        readingReaders--;
        preferWriter = true;
        notifyAll();
    }

    synchronized void writeLock() throws InterruptedException{
        waitingWriters++;
        try {
            while(readingReaders > 0 || writingWriters > 0) {
                wait();
            }
        } finally {
            waitingWriters--;
        }
        writingWriters++;
    }

    synchronized void writeUnlock() {
        writingWriters--;
        preferWriter = false;
        notifyAll();
    }
}

public class Test {
    public static void main(String[] args) {
        var data = new Data(10);
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new ReaderThread(data).start();
        new WriterThread(data, "ABCDEF").start();
        new WriterThread(data, "abcdef").start();
    }
}
```