---
title: Java 17 - NIO
date: 2018-07-30 01:20:07
categories: Java
---
# NIO

<!--more-->

## 概念

我们传统上处理socket IO时, 需要给每一个连接创建一个线程, 当并发的连接数量非常大的时候, 线程所占用的栈内存和CPU线程切换的开销将非常大. 如果使用NIO, 那么不需要为每一个连接创建一个单独的线程, 可以用一个含有有限数量线程的线程池, 甚至是一个线程来为任意数量的连接提供服务. 由于线程数量小于连接数量, 那么就决定了IO的工作模式不能是阻塞, 否则有些连接就会被饿死.

## 原理

1. 由一个专门的线程来处理所有的IO事件, 并负责转发.
2. 事件驱动机制: 事件到的时候触发, 而不是同步的去监视事件.
3. 线程通讯: 线程之间通过wait, notify等方式通讯, 保证每次上下文切换是有意义的, 减少无畏的线程切换.

NIO实现了IO多路复用中的Reactor模型, 一个线程使用Selector通过轮询的方式去监听多个Channel上的时间, 从而使一个线程可以处理多个事件.

注意的是, 只有为SocketChannel才能配置非阻塞, 而为FileChannel不能, 况且这也没有意义.

## 注册Channel

当我们将Channel注册到Selector上时, 要指定注册的具体事件, 事件的定义如下:

```java
public static final int OP_READ = 1 << 0;
public static final int OP_WRITE = 1 << 2;
public static final int OP_CONNECT = 1 << 3;
public static final int OP_ACCEPT = 1 << 4;
```

我们将事件状态压缩, 这样可以通过|来表示多个事件类型.

## 优点

总的来说, NIO对比BIO有如下优点:

1. 节约了大量线程所占用的栈空间.

2. 避免线程之间的切换所带来的花费.

3. 数据块为单位的IO速度高于传统的流式读取速度.

## 实例

```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;

class NIOServer {
    private static String readDataFromSocketChannel(SocketChannel socketChannel) throws IOException {
        ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
        StringBuilder data = new StringBuilder();
        while(true) {
            byteBuffer.clear();
            int n = socketChannel.read(byteBuffer);
            if(n == -1) {
                break;
            }
            byteBuffer.flip();
            int limit = byteBuffer.limit();
            char[] currentData = new char[limit];
            for(int i = 0; i < limit; i++) {
                currentData[i] = (char) byteBuffer.get(i);
            }
            data.append(currentData);
        }
        return data.toString();
    }

    public static void main(String[] args) throws IOException {
        Selector selector = Selector.open();

        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        serverSocketChannel.configureBlocking(false);
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

        ServerSocket serverSocket = serverSocketChannel.socket();
        InetSocketAddress inetSocketAddress = new InetSocketAddress("127.0.0.1", 8888);
        serverSocket.bind(inetSocketAddress);

        while(true) {
            selector.select();
            Set<SelectionKey> keys = selector.selectedKeys();
            Iterator<SelectionKey> keyIterator = keys.iterator();
            while(keyIterator.hasNext()) {
                SelectionKey selectionKey = keyIterator.next();
                if(selectionKey.isAcceptable()) {
                    ServerSocketChannel serverSocketChannel1 = (ServerSocketChannel) selectionKey.channel();
                    SocketChannel socketChannel = serverSocketChannel.accept();
                    socketChannel.configureBlocking(false);
                    socketChannel.register(selector, SelectionKey.OP_READ);
                } else if(selectionKey.isReadable()) {
                    SocketChannel socketChannel = (SocketChannel) selectionKey.channel();
                    System.out.println(readDataFromSocketChannel(socketChannel));
                    socketChannel.close();
                }
                keyIterator.remove();
            }
        }
    }
}

class NIOClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("127.0.0.1", 8888);
        OutputStream outputStream = socket.getOutputStream();
        String string = "Hello World!";
        outputStream.write(string.getBytes());
        outputStream.close();
    }
}
```