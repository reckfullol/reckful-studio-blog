---
title: Operating System 14 - 分支预测分析
date: 2021-05-17 20:32:50
categories: Operating System
---
# 分支预测分析

<!--more-->

## 背景
业务中在高频调用代码段会出现条件判断语句, 因此联想cpu架构中的分支预测功能, 进行简要分析.

### 指令流水线
指令流水线通过将指令拆分为若干个连续, 独立的步骤来提升单位时间内同时执行的指令数(吞吐量).

#### 流水线/非流水线对比
- 流水线架构: 基于MIPS架构的CPU**五级流水线**, 在一个时钟周期最多可以同时处理五个独立步骤:
	- a: 读取指令(Instruction Fetch).
	- b: 解码指令和读取寄存器(Instruction Decode and Register Fetch).
	- c: 算术运算(Arithmetic Computation).
	- d: 内存访问(Memory Read or Write).
	- e: 回写寄存器(Register Write).
- benchmark: 9条相同简单**指令**.
- 测试结果[1]:
	- 非流水线模式:![image.png](https://i.loli.net/2021/05/17/q5rSsMhmFl9vbEx.png)
	- 流水线模式:![image.png](https://i.loli.net/2021/05/17/YK31dDcM2EejBVv.png)
- 分析:
非流水线模式**后继步骤需要等待前序步骤的完成**, 模块大部分时间处于idle状态.
比较相同9个简单指令在不同模式完成时间, **非流水线模式需要40个时钟周期(200ns), 流水线模式需要13个时钟周期(65ns)**.![image.png](https://i.loli.net/2021/05/17/eU4ZntQCX6sfiE2.png)

### 分支预测
流水线能够在同一个时钟周期处理多个步骤的充分条件是: **每个步骤相互独立, 不存在依赖关系**. 指令流水时, 处理器遇到分支指令, 不能在开始阶段就判断出分支结果, 即**控制冒险(分支冒险)**.
避免控制冒险的方法:
	1.  在分支指令后插入流水线冒泡, 直到分支指令的流水执行完毕.![image.png](https://i.loli.net/2021/05/17/KqLEbDRBmlpdPe9.png)
	2. 使用**分支预测**(在分支指令执行结束之前猜测哪一路分支将会被运行), 然后**投机执行**. 如果分支预测失败, 则要有能力**恢复到分支指令执行完毕时刻的寄存器状态**, 进入正确的分支继续执行.

分支预测分为两个大类: **静态预测**和**动态预测**:
	1. 静态预测: 无论执行什么指令, 分支预测器总是执行相同的预测策略(无状态).
	2. 动态预测: 会根据执行指令的不同, 依据program counter(PC)的值以及历史信息等做出不同的预测(有状态).

#### 分支预测策略对比
- 分支预测策略:
	1. 静态预测:
		- Strategy 1: 预测所有分支都会跳转(Predict that all branches will be taken).
		- Strategy 1a: 某些指令一律跳转, 其余指令一律不跳转, 例如判断大于等于操作一律跳转(Predict that all branches with certain operation codes will be taken; predict that the others will not be taken.).
		- Strategy 3: 预测向低地址空间的分支跳转, 向高地址空间的分支不跳转. 地址指地址空间前后, 主要场景是每当到了一个循环的末尾判断是否继续循环时会预测向前跳转继续循环(Predict that all backward branches (toward lower addresses) will be taken; predict that all forward branches will not be take).
	2. 动态预测:
		- Strategy 2: 做出和上次是否跳转一样的预测, 默认跳转(Predict that a branch will be decided the same way as it was on its last execution. If it has not been previously executed, predict that it will be taken).
		- Strategy 4: 维护没有分支跳转的表, 如果当前分支在表中, 预测不跳转, 反之预测跳转. 如果实际跳转, 则从表中移除该项, 该表遵循LRU(Maintain a table of the most recently used branch instructions that are not taken. If a branch instruction is in the table, predict that it will not be taken; otherwise predict that it will be taken. Purge table entries if they are taken, and use LRU replacement to add new entries).
		- Strategy 5: 在cache中用bit表记录分支指令的跳转结果, 预测当前分支指令与一致, 默认跳转, bit表遵循LRU(Maintain a bit for each instruction in the cache. If an instruction is a branch instruction, the bit is used to record if it was taken on its last execution. Branches are predicted to be decided as on their last execution; if a branch has not been executed, it is predicted to be taken (implemented by initializing the bit cache to taken when an instruction is first placed in cache)).
		- Strategy 6: 在RAM中用哈希表维护分支指令的跳转结果, 预测当前分支结果与上次一致(Hash the branch instruction address to m bits and use this index to address a random access memory containing the outcome of the most recent branch instruction indexing the same location. Predict that the branch outcome will be the same).
		- Strategy 7: 基于Strategy 6将表项内容替换为2-bit状态机(saturated counter), 初始值为00, 每当分支跳转则加1, 反之减一, 根据首位预测当前分支跳转.![image.png](https://i.loli.net/2021/05/17/ZIjTKWmuLENH4Ca.png)
- benchmark:
	1. ADVAN : 计算三个联立偏微分方程的解(Calculates the solution of three simultaneous partial differential equations).
	2. SCI2: 计算矩阵求逆(Performs matrix inversion).
	3. SINCOS: 将一系列点从极坐标转换为笛卡尔坐标(Converts a series of points from polar to Cartesian coordinates).
	4. SORTST: 对10,000个整数进行希尔排序(Sorts a list of 10,000 integers using the shell sort algorithm).
	5. GIBSON: 模拟GIBSON max算法(An artificial program that compiles to instructions that roughly satisfy the so called GIBSON mix).
	6. TBLLNK: 大量条件判断下进行链表操作(Processes a linked list and contains a variety of conditional branches).
- 测试结果[2]:![image.png](https://i.loli.net/2021/05/17/OwGlZKTgQeHhYqn.png)
	平均分支预测成功率86%, 最大分支预测成功率99.4%, 说明主流分支预测策略对于常见逻辑运算有不错表现.![image.png](https://i.loli.net/2021/05/17/lFmkxyjY9hnuq7v.png)

## 测试
- CPU: Intel(R) Xeon(R) CPU E5-2670 v3 @ 2.30GHz(perf 2.6GHz).
- benchmark: 基于数组的条件遍历, 数组元素[0, 255], 通过分支执行条件`data[c] > number`控制分支执行概率[benchmark](https://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-processing-an-unsorted-array).
### 有序数组/无序数组对比
- benchmark: 3.2e9有序/无序数组条件遍历求和, 分支执行概率**50%**.
- 测试结果:![image.png](https://i.loli.net/2021/05/17/gpZ78lP1VqOEywr.png)
- 分析:
	1. 有序数组分支预测成功率提高24.48%, **接近100%**. 说明**分支预测成功率与逻辑局部性显著性相关**, 测试CPU基于**动态预测策略**.
	2. 任务耗时倒数比与IPC比, 分支处理速度比基本一致. 说明**测试程序执行效率主要取决于分支处理速度, 根本上取决于流水线的并行程度**. 
	3. 平均单次分支预测失败增加任务耗时(26.46 - 9.09) \/ (1608893451 - 441722)  = 1.07992e-08s = **10.7992ns**, 增加时钟周期1.07992e-08 \/ (1 / 2.6e9) = **28个**.

### IF/ELSE对比
- benchmark: 3.2e9无序数组条件遍历求和**逻辑分别运行在IF和ELSE分支**, 分支执行概率80%.
- 测试结果:![image.png](https://i.loli.net/2021/05/17/KIjfnNTiZpxdPc7.png)
- 分析:
	1. 任务耗时, IPC和分支预测成功率近似相等, 说明**逻辑放置IF或ELSE分支对性能无明显影响**.
￼
### 分支执行概率对比
- benchmark: 3.2e9无序数组条件遍历求和, 分支执行概率**{50%, 60 %, 70%, 80%, 90%, 100%}**.
- 测试结果:![image.png](https://i.loli.net/2021/05/17/CkBpUNxPqaMw93h.png)
- 分析:
	1. 随着分支执行概率增加, 任务耗时线性减少; IPC, 分支处理速度和分支预测成功率线性增加.
	2. 各项参数与分支执行概率进行**相关性分析**, 发现与分支执行概率**显著性相关**.![image.png](https://i.loli.net/2021/05/17/DGQskV6KdE27R3C.png)

### 有无分支判断语句对比
- benchmark: 3.2e9无序数组条件遍历求和, 比较分支执行概率**100%**与无分支判断语句.
- 测试数据:![image.png]( https://i.loli.net/2021/05/17/OVLQvMAJpmuUqIw.png)
- 分析:
	1. 增加分支逻辑语句后, 指令数增加9831210237次. 分支逻辑调用次数增加3276800000次(数组元素个数), 做比得到9831210237 \/ 3276800000 = 3. **猜想测试程序中单次分支逻辑相当于执行三次CPU指令**. 我们比较C++文件差异和汇编文件差异:
		1. C++文件差异:
```diff
unsorted_with_if.cpp, unsorted_without_if.cpp
<             if (data[c] >= 0)
<             {
<                 sum += data[c];
<             }
---
>             sum += data[c];
```
		2. 汇编文件差异:
```diff
! .L7:
<   testl   %eax, %eax
<   js  .L6							; 实际逻辑不会执行分支跳转
<   movl	-24(%rbp), %eax
<	  movl	-131120(%rbp,%rax,4), %eax ; %eax = data[c]
    cltq								; cltq = movslq %eax, %rax; %rax = data[c]
    addq    %rax, -16(%rbp)			; sum += data[c]
- .L6:
    addl    $1, -24(%rbp)
```
		3. 因为逻辑控制分支保证始终跳转, 因此实际增加的汇编指令调用为:
```nasm
testl   %eax, %eax
movl	-24(%rbp), %eax
movl	-131120(%rbp,%rax,4), %eax
```
		**实际分支逻辑添加导致汇编指令数量增加了3条, 与猜想一致**(汇编指令集与CPU指令集基本对应).
	2. 删除分支逻辑语句后流水线并行程度下降, 但程序指令数减少, **整体任务耗时降低**.
	3. 平均单次分支逻辑语句增加任务耗时 (2.18 - 1.69) \/ 3276800000 = 1.49536e-10s = **0.149536ns**, 增加指令周期 1.49536e-10 \/ (1 \/ 2.6e9) = **0.38个**. 因为分支预测成功率为100%, 所以说明**分支预测成功下平均单次分支逻辑语句的性能开销极低**.![image.png](https://i.loli.net/2021/05/17/WMUiEuXe8PgO6cz.png)

### __builtin_expect对比
- benchmark: 3.2e9无序数组条件遍历求和, 分别测试有无[__builtin_expect](https://www.ibm.com/docs/en/zos/2.2.0?topic=performance-builtin-expect)内置函数, 分支执行概率**80%**.
```cpp
if(__builtin_expect(!!(data[c] > number), true))
```
- 测试数据:![image.png](https://i.loli.net/2021/05/17/BVPw35pi2M4QcNG.png)
- 分析:
	1. 任务耗时, IPC和分支预测成功率近似相等, 说明有无`__builtin_expect`内置函数对性能无明显影响.
	2. 查看插入__builtin_expect后汇编文件:
```diff
	movl	-24(%rbp), %eax
	movl	-131120(%rbp,%rax,4), %eax
	cmpl	$50, %eax
<	setg	%al; 			取出cmp结果
<	movzbl	%al, %eax; 		%eax = cmp结果
<	testq	%rax, %rax; 	cmp $0, %rax
	je	.L6
	movl	-24(%rbp), %eax
	movl	-131120(%rbp,%rax,4), %eax
	cltq
	addq	%rax, -16(%rbp)
```
	`__builtin_expect`并**无实质逻辑优化**.

### 编译器优化等级对比
- benchmark: 3.2e9无序数组条件遍历求和, 分别测试不同编译器优化等级, 分支执行概率**50%**.
- 测试数据:![image.png](https://i.loli.net/2021/05/17/2vLnq9jTzXkhHUp.png)
- 分析:
	1. 随着编译优化等级提高, 任务耗时减少; IPC, 分支处理速度和分支预测成功率提高.
	2. 任务耗时有两次明显下降:![image.png](https://i.loli.net/2021/05/17/sdBc1OfiHjr4bUA.png)
		1. O0 -> O1: 任务耗时降低41%, 分支预测成功率无明显变化, 任务耗时反比为1 \/ (15.58 \/ 26.46) = 1.69833, 分支处理速度比为 420.85 \/ 248.17 = 1.69581. 说明**O1阶段编译器主要优化分支处理速度**.![image.png](https://i.loli.net/2021/05/17/bgh7Dx69BT2GXkf.png)
		2. O2 -> O3: 任务耗时降低75%, 分支预测成功率提高24.5%至99.99%.![image.png](https://i.loli.net/2021/05/17/MxIAR8rtEKqQkSV.png)
		O3汇编代码:
```nasm
.L7:
    movl    (%rdx), %ecx
    movslq  %ecx, %rsi ; %rsi = data[c]
    addq    %rbx, %rsi ; %rsi += sum
    cmpl    $127, %ecx ; 127 - data[c]
    cmovg   %rsi, %rbx ; sum = %rsi
; 指令重排, 占用%rsi寄存器临时存储结果; cmovg条件赋值%rsi给sum(做差结果存储在RFLAGS寄存器)
```
		分析发现:
			1. 通过**指令重排**预先计算分支内结果, 暂存在寄存器中.
			2. 使用**CMOVG**汇编指令, 根据RFLAGS标志条件执行, 规避因跳转而可能引起的分支预测陷阱, 使其分支预测成功率接近100%.![image.png](https://i.loli.net/2021/05/17/pi69hWLUdbHSslP.png)

### BitHack对比
- benchmark: 3.2e9无序数组条件遍历求和, 分别测试条件分支语句与[BitHack](http://graphics.stanford.edu/~seander/bithacks.html)实现, 分支执行概率**50%**:
	BitHack C++:
```diff
		< int t = (data[c] - 128) >> 31;
		< sum += ~t & data[c];
		> if (data[c] >= 128)
		>    sum += data[c];
```
	**条件判断结果做差方式存储在标志位t中, ~t &的方式条件操作**:
		- data[c] >= 128: t = 0, ~0 & n = n.
		- data[c] < 128: t = -1; ~-1 & n = 0.
- 测试数据:![image.png](https://i.loli.net/2021/05/17/U72ejFLk9nEmxco.png)
- 分析:
	1. BitHack与有序数组O3优化, 无序数组O3优化性能开销基本相同.![image.png](https://i.loli.net/2021/05/17/RiHWtNB8AbEVuUs.png)

## 总结
1. 现代分支预测器在通用场景下的分支预测成功率平均值在70%以上, 逻辑局部性越明显, 分支预测成功率越高.
2. 逻辑放置在IF逻辑分支还是ELSE逻辑分支对性能无明显影响.
3. `__builtin_expect`内置函数对分支预测成功率无明显影响.
4. 不同分支预测结果对分支逻辑执行效率的影响: 
	1. 分支预测成功: 平均任务耗时0.149536ns, 指令周期0.38个. 
	2. 分支预测失败: 平均任务耗时10.7992ns, 指令周期28个.
	3. 分支预测失败开销是分支预测成功的72倍.![image.png](https://i.loli.net/2021/05/17/zjfdEOe1siBr2bp.png)
5. 提高分支预测准确率的优化方法:
	1. 增强逻辑局部性, 增大数据倾斜程度.
	2. 使用`-O3`编译优化等级或开启`-ftree-vectorize`编译优化选项, 生成条件移动汇编指令.
	3. 逻辑代码中模拟**CMOV**, 避免显式分支逻辑语句出现.

## 引用
- [1] Filsinger, M. D. (2005). Understanding CPU Pipelining Through Simulation Programming. *age*, *10*, 1.
- [2] Smith, J. E. (1998, August). A study of branch prediction strategies. In *25 years of the international symposia on Computer architecture (selected papers)* (pp. 202-215).
