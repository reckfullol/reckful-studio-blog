---
title: CUDA 01 - 硬件架构
date: 2019-02-28 11:19:15
categories: CUDA
---
# 硬件架构

<!--more-->

## SP

![SM](https://res.cloudinary.com/dpe4i978o/image/upload/v1551324276/cuda/SM.jpg)

SP(Streaming Processor): 也称为CUDA Core, 是任务执行的基本单元, GPU的并行计算就是多个SM同时进行计算.

## SM

![gpu-hardware-structure](https://res.cloudinary.com/dpe4i978o/image/upload/v1551325275/cuda/gpu-hardware-structure.png)

SM(Streaming Multiprocessor): 由多个SP加上warp scheduler, register, shared memory等资源构成. 和CPU类似, register/shared memory是SM的稀缺资源, 供给驻留线程的使用, 因此也限制了GPU的并行能力.

## SIMT

![SIMT](https://res.cloudinary.com/dpe4i978o/image/upload/v1551326085/cuda/SIMT.png)

SIMT: 具有Tesla架构的GPU具有一组SIMT(Single Instruction, Multiple Thread)多处理器. 他以可伸缩的SMs(Streaming Processors)阵列为中心实现了MIMD(Multiple instruction, Multiple Thread)的异步并行机制, 其中每个多处理器都包含了多个SP(Scale Processor), 为了管理运行各种不同程序的数百个线程, SIMT架构的多处理器会将各个线程映射到一个SP核心, 各个线程使用自己的指令地址和寄存器状态独立执行.

每个MP(Mpltiple Processor)都拥有下列四种存储空间:

1. Register: 本地32位的寄存器.
2. Shared Memory: 并行数据缓存或共享存储器, 由所有SP核心共享.
3. Constant Memory: 加速从固定存储空间进行的读取操作(只读), 由所有SP核心共享.
4. Texture Memroy: 加速从纹理存储空间进行的读取操作(只读), 每个MP都会通过实现不同寻址模型和数据过滤的纹理单元来访问纹理缓存, 由所有SP核心共享.

## Warp

[warp-status](https://res.cloudinary.com/dpe4i978o/image/upload/v1551327738/cuda/warp-status.png)

SIMT以32个并行线程作为创建, 管理, 调度和执行的基本单位, 这样的线程组被称为warp. 当主机CPU上的CUDA程序调用到内核网格的时候, 网格的块将被枚举分发到具有可用执行容量的MP, SIMT会选择一个已经准备好的warp块, 并将下一条指令发送到这个warp块的活动进程. 一个warp的各个线程会在一个MP上并发执行.
