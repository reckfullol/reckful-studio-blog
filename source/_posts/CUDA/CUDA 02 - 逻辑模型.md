---
title: CUDA 02 - 逻辑模型
date: 2019-02-28 15:53:23
categories: CUDA
---
# 逻辑模型

<!--more-->

CUDA逻辑模型是异构模型, 需要CPU和GPU协同工作. 在CUDA中, host和device是两个重要概念, host是指CPU及其内存, device是指GPU及其内存. 典型的CUDA程序的执行流程如下:

1. 分配host, 并进行数据初始化
2. 分配device内存, 并从host将数据拷贝到device上.
3. 调用CUDA的和函数在device上完成指定的运算.
4. 将device上的运算结果拷贝到host上.
5. 释放device和host上分配的内存.

kernel是在device上并行执行的函数, 在调用此类函数时, 将由N个不同的CUDA线程并行执行N次, 执行kernel的每个线程都会被分配一个唯一的线程ID, 可以通过threadIdx变量在内核中辨别线程.

在CUDA程序中, 主程序在调用任何GPU内核之前, 必须对核进行配置, 以确定线程块数和每个线程块中的线程数以及共享内存大小.

## 线程层级结构

![grid](https://res.cloudinary.com/dpe4i978o/image/upload/v1551341913/cuda/grid.png)

一个kernel所启动的所有线程被称为一个grid, 同一个grid上的线程共享相同的全局内存空间. grid是线程结构的第一层次, grid又可以分为很多block, 一个block里包含很多thread, 这是第二个层次. grid和block都是定义为dim3类型的变量, dim3可以看成是包含三个无符号整数(x, y, z)成员的结构体变量, 在定义时缺省为1. 因此grid和block可以灵活的定义为1-dim, 2-dim以及3-dim结构.

```c
// 上图kernel结构定义

dim3 grid(2, 3);
dim3 block(3, 4);
kernel_func<<<grid, block>>>(params);
```

由于CUDA是异构模型, 所以需要区分host和device上的代码, 在CUDA中通过函数修饰限定词来区分的: 主要三种限定词如下:

1. `__global__`: 在device上执行, 从host中调用, 返回类型必须是void, 不支持可变参数, 不能成为类成员函数. 注意使用`__global__`定义的kernel是异步的, 这意味着host不会等待kernel执行完就执行下一步. 其调用形式为: kernel_func<<<Dg, Db, Ns, s>>>(param list):
  
    - Dg: 定义grid维度和尺寸.
    - Db: 定义block维度和尺寸.
    - Ns: 可选参数, 设置每个block除静态分配的Shared Memory外, 最多能动态分配的Shared Memory大小, 单位为byte. 缺省值为0.
    - S: 可选参数, 表示该kernel处于哪个cuda stream中, 缺省值为0.

2. `__device__`: 在device上执行, 仅可以从device中调用, 不可以和`__global__`同时用.

3. `__host__`: 在host上执行, 仅可以从host上调用, 一般省略不写, 不可以和`__global__`同时用, 但可以和`__device__`同时用, 此时函数会在device和host上都编译.

## 向量加法实例

```cpp

#include "cuda_runtime.h"
#include "device_launch_parameters.h"

#include <stdio.h>

cudaError_t addWithCuda(int *c, const int *a, const int *b, unsigned int size);

__global__ void addKernel(int *c, const int *a, const int *b)
{
    int i = threadIdx.x;
    c[i] = a[i] + b[i];
}

int main()
{
    const int arraySize = 5;
    const int a[arraySize] = { 1, 2, 3, 4, 5 };
    const int b[arraySize] = { 10, 20, 30, 40, 50 };
    int c[arraySize] = { 0 };

    // Add vectors in parallel.
    cudaError_t cudaStatus = addWithCuda(c, a, b, arraySize);
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "addWithCuda failed!");
        return 1;
    }

    printf("{1,2,3,4,5} + {10,20,30,40,50} = {%d,%d,%d,%d,%d}\n",
        c[0], c[1], c[2], c[3], c[4]);

    // cudaDeviceReset must be called before exiting in order for profiling and
    // tracing tools such as Nsight and Visual Profiler to show complete traces.
    cudaStatus = cudaDeviceReset();
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaDeviceReset failed!");
        return 1;
    }

    return 0;
}

// Helper function for using CUDA to add vectors in parallel.
cudaError_t addWithCuda(int *c, const int *a, const int *b, unsigned int size)
{
    int *dev_a = 0;
    int *dev_b = 0;
    int *dev_c = 0;
    cudaError_t cudaStatus;

    // Choose which GPU to run on, change this on a multi-GPU system.
    cudaStatus = cudaSetDevice(0);
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaSetDevice failed!  Do you have a CUDA-capable GPU installed?");
        goto Error;
    }

    // Allocate GPU buffers for three vectors (two input, one output)    .
    cudaStatus = cudaMalloc((void**)&dev_c, size * sizeof(int));
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaMalloc failed!");
        goto Error;
    }

    cudaStatus = cudaMalloc((void**)&dev_a, size * sizeof(int));
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaMalloc failed!");
        goto Error;
    }

    cudaStatus = cudaMalloc((void**)&dev_b, size * sizeof(int));
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaMalloc failed!");
        goto Error;
    }

    // Copy input vectors from host memory to GPU buffers.
    cudaStatus = cudaMemcpy(dev_a, a, size * sizeof(int), cudaMemcpyHostToDevice);
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaMemcpy failed!");
        goto Error;
    }

    cudaStatus = cudaMemcpy(dev_b, b, size * sizeof(int), cudaMemcpyHostToDevice);
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaMemcpy failed!");
        goto Error;
    }

    // Launch a kernel on the GPU with one thread for each element.
    addKernel<<<1, size>>>(dev_c, dev_a, dev_b);

    // Check for any errors launching the kernel
    cudaStatus = cudaGetLastError();
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "addKernel launch failed: %s\n", cudaGetErrorString(cudaStatus));
        goto Error;
    }
    
    // cudaDeviceSynchronize waits for the kernel to finish, and returns
    // any errors encountered during the launch.
    cudaStatus = cudaDeviceSynchronize();
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaDeviceSynchronize returned error code %d after launching addKernel!\n", cudaStatus);
        goto Error;
    }

    // Copy output vector from GPU buffer to host memory.
    cudaStatus = cudaMemcpy(c, dev_c, size * sizeof(int), cudaMemcpyDeviceToHost);
    if (cudaStatus != cudaSuccess) {
        fprintf(stderr, "cudaMemcpy failed!");
        goto Error;
    }

Error:
    cudaFree(dev_c);
    cudaFree(dev_a);
    cudaFree(dev_b);
    
    return cudaStatus;
}

```