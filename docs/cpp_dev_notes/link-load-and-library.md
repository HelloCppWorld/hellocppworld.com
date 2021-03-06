# 《程序员的自我修养——链接、装载与库》读书笔记

## 第1部分 简介

### 第1章 温故而知新

#### 1.1 从Hello World说起

```c
#include <stdio.h>
int main()
{
	printf("Hello world\n");
	return 0;
}
```

* 程序为什么要编译之后才能运行？
* 编译器如何把C语言转换成机器码？
* 编译出来的可执行文件里存放的是什么？除了机器码还有什么？如何存放？如何组织？
* 头文件是什么？把头文件包含进来意味着什么？C语言库是什么？如何实现？
* 不同的编译器（MSVC、GCC）和不同的硬件平台（x86、SPARC、MIPS、ARM），以及不同的操作系统（Windows、Linux、Unix、Solaris），最终编译出来的结果一样吗，为什么？
* Hello World程序是如何运行起来的？操作系统是怎么装载它的？它从哪儿开始执行，到哪儿结束？`main`函数之前发生了什么？`main`函数之后又发生了什么？
* 如果在一台没有操作系统的机器上运行Hello World需要什么，应该如何实现？
* `printf`是怎么实现的？它为什么可以又不定数量的参数？为什么它能够在终端上输出字符串？
* Hello World在程序程序运行时，它在内存中是什么样子？

编译、静态链接、操作系统如何装载程序、动态链接、运行库和标准库的实现。

#### 1.2 万变不离其宗

计算机硬件三个关键部件：CPU、内存、I/O控制芯片

##### SMP与多核

SMP：Symmetrical Multi-Processing 对称多处理器，多个物理CPU，每个CPU都相互独立

MCP： Multi-core Processor 多核处理器，多个核心集成在一块物理CPU内

两者在软件层面基本相同，无需做特别区分。

#### 1.3 站得高，望得远

> Any problem in computer science can be solved by anther layer of indirection.
>
> 任何计算机科学领域的问题都可以通过增加一个中间层来解决。

#### 1.4 操作系统做什么

挖掘CPU、存储器和I/O设备的潜力。

##### 1.4.1 不要让CPU打盹

分时系统（Time-Sharing System）Windows 95/Mac OS 程序自动出让CPU资源

多任务系统（Multi-tasking）CPU分配采用抢占式（Preemptive）操作系统强制剥夺程序资源

##### 1.4.2 设备驱动

管理和抽象硬件。屏蔽硬件底层千差万别的实现细节，提供统一接口给硬件使用者使用。

#### 1.5 内存不够怎么办

早起计算机中，程序直接运行在物理内存上。这种简单的内存分配策略问题很多：

- 地址空间不隔离
- 内存使用效率低
- 程序运行的地址不确定

增加中间层可以解决这个问题：把程序地址当做虚拟地址（Virtual Address），然后通过映射的方法转换成物理地址。

##### 1.5.1 关于隔离

地址分为两种，虚拟地址空间和物理地址空间

每个进程所感知的是完整的4G（32位系统）内存地址空间，这个地址叫虚拟地址（线性地址）。

而物理内存的真实地址空间可能小于4G，也可能大于4G（大概率64位系统）。

每个进程所感知的地址空间与其他进程所感知的地址空间虽然相同，但是映射到物理内存却不相同。

那么进程1对地址空间`0x12345678`的更改不会影响到进程2的地址空间`0x12345678`。

##### 1.5.2 分段（Segmentation）

分段是把程序所需的内存空间直接映射到物理空间。不同进程有访问相同地址，加上不同偏移，访问的物理地址就不相同。虽然进程隔离与内存地址不确定问题，但是内存利用率低无法解决。

##### 1.5.3 分页（Paging）

将虚拟地址（32位4G）按照每页4K（通常）大小分成2^20个页。物理空间也是同样的分法。

``` text
逻辑地址 + 段基址 --> 虚拟地址（线性地址）--> MMU --> 物理地址
```

#### 1.6 众人拾柴火焰高

##### 1.6.1 线程基础

##### 什么是线程

线程（Thread）优势被称为轻量级进程（Lightweight Process,LWP），是程序执行的最小单元，也是CPU调度的最小单元。

同一个进程内的线程共享代码段、数据段、堆、信号、打开的文件等资源，每个线程拥有各自的ID、指令指针（PC）、寄存器组的值（共享寄存器组）、堆栈、返回的错误码、信号屏蔽码、优先级。

##### 线程的访问权限

线程私有存储空间：

+ 栈
+ 线程局部存储（Thread Local Storage, TLS）
+ 寄存器（包括PC寄存器）

线程之间共享（进程所有）：

+ 全局变量
+ 堆上的数据
+ 函数里的静态变量
+ 程序代码
+ 打开的文件

##### 线程调度与优先级

线程至少拥有三种状态：

+ 运行（Running）
+ 就绪（Ready）
+ 等待（Waiting）

线程调度方式：所有主流的调度算法都带有**优先级调度（Priority Schedule）**和**轮转法（Round Robin）**。

改变优先级的三种方式：

+ 用户制定优先级
+ 根据进入等待状态的频繁程度提升或降低优先级
+ 长时间得不到执行而被提升优先级

##### 可抢占线程和不可抢占线程

可抢占（Preemptive）线程允许操作系统强制终止当前线程执行，释放CPU资源。

不可抢占线程不允许操作系统强制终止当前线程执行，只能由线程主动出让CPU资源。

##### Linux的多线程

Windows对于进程和线程支持的较为标准。

Linux内核中并不存在真正的意义上的线程。Linux将所有的执行实体（进程或线程）都称为**任务（Task）**。

不同的任务可以选择共享内存空间，共享内存空间的多个任务构成进程，这些任务就成了线程。

Linux创建任务：

+ fork 复制当前进程
+ exec 使用新的可执行映像覆盖当前可执行映像
+ clone 创建子进程并从指定位置开始执行

##### 1.6.2 线程安全

##### 竞争和原子操作

自增（++）在多线程环境下不是线程安全的。

WIndows和Linux各自提供了一套原子操作。使用这些原子操作自增或者自减是线程安全的。

Windows: Interlocked

Linux: atomic

##### 同步与锁

同步（Synchronization）：一个线程访问数据未结束的时候，其他线程不得对同一数据进行访问。如此，对数据的访问被**原子化**了。

同步最常见的方法是使用**锁（Lock）**，每个线程在访问数据时视图获取（Acquire）锁，并在结束之后释放（Release）锁。在锁已经被占用时试图获取锁，线程会一直等待，知道锁被释放。

**二元信号量（Binary Semaphore）**是最简单的锁，它只有**占用**和**非占用**两种状态。可以用二元信号量实现锁的功能。

多元信号量简称信号量（Semaphore），初始值为N的信号量允许N个线程同时并发访问。

互斥量（Mutex）和二元信号量类似，仅允许一个线程访问保护的资源。与信号量不同的时，互斥量的获取与释放需要在一个线程中完成，而互斥量可以跨线程使用。

**临界区（Critical Section）**是比互斥量更严格的同步手段。



##### 1.6.3 多线程内部情况

#### 1.7 本章小结

## 第2部分 静态链接
### 第2章 编译和链接
#### 2.1 被隐藏了的过程
##### 2.1.1 预编译
##### 2.1.2 编译
##### 2.1.3 汇编
##### 2.1.4 链接
#### 2.2 编译器做了什么
##### 2.2.1 词法分析
##### 2.2.2 语法分析
##### 2.2.3 语义分析
##### 2.2.4 中间语言生成
##### 2.2.5 目标代码生成与优化
#### 2.3 链接器年龄比编译器长
#### 2.4 模块拼装——静态链接
#### 2.5 本章小结
### 第3章 目标文件里有什么
#### 3.1 目标文件的格式
#### 3.2 目标文件是什么样的
#### 3.3 挖掘SimpleSection.o
##### 3.3.1 代码段
##### 3.3.2 数据段和只读数据段
##### 3.3.3 BSS段
##### 3.3.4 其他段
#### 3.4 ELF文件结构描述
##### 3.4.1 文件头
##### 3.4.2 段表
##### 3.4.3 重定位表
##### 3.4.4 字符串表
#### 3.5 链接接口——符号
##### 3.5.1 ELF符号表结构
##### 3.5.2 特殊符号
##### 3.5.3 符号修饰与函数签名
##### 3.5.4 extern “C”
##### 3.5.5 弱符号与强符号
#### 3.6 调试信息
#### 3.7 本章小结
### 第4章  静态链接
#### 4.1 空间与地址分配
##### 4.1.1 按序叠加
##### 4.1.2 相似段合并
##### 4.1.1 符号地址的确定
#### 4.2 符号解析与重定位
##### 4.2.1 重定位
##### 4.2.2 重定位表
##### 4.2.3 符号解析
##### 4.2.4 指令修正方式
#### 4.3 COMMON块
#### 4.4 C++相关问题
##### 4.4.1 重复代码消除
##### 4.4.2 全局构造与析构
##### 4.4.3 C++与ABI
#### 4.5 静态块链接
#### 4.6 链接过程控制
##### 4.6.1 链接控制脚本
##### 4.6.2 最“小”的程序
##### 4.6.3 使用ld链接脚本
##### 4.6.4 ld链接脚本语法简介
#### 4.7 BFD库
#### 4.8 本章小结
### 第5章 Windows PE/COFF
#### 5.1 WIndows的二进制文件格式PE/COFF
#### 5.2 PE的前身——COFF
#### 5.3 链接指示信息
#### 5.4 调试信息
#### 5.5 大家都有符号表
#### 5.6 Windows下的ELF——PE
##### 5.6.1 PE数据目录
#### 5.7 本章小结
## 第3部分
### 第6章 可执行文件的装载与进程
#### 6.1 进程虚拟地址空间
#### 6.2 装载的方式
##### 6.2.1 覆盖装入
##### 6.2.2 页映射
#### 6.3 从操作系统角度看可执行文件的装载
##### 6.3.1 进程的建立
##### 6.3.2 页错误
#### 6.4 进程虚拟空间分布
##### 6.4.1 ELF文件链接视图和执行视图
##### 6.4.2 堆和栈
##### 6.4.3 堆的最大申请数量
##### 6.4.4 段地址对齐
##### 6.4.5 进程栈初始值
#### 6.5 Linux内核装载ELF过程简介
#### 6.6 Windows PE的装载
#### 6.7 本章小结
### 第7章 动态链接
#### 7.1 为什么要动态链接
#### 7.2 简单的动态链接的例子
#### 7.3 地址无关代码
##### 7.3.1 固定装载地址的困扰
##### 7.3.2 装载时重定位
##### 7.3.3 地址无关代码
##### 7.3.4 共享模块的全集变量问题
##### 7.3.5 数据段地址无关性
#### 7.4 延迟绑定（PTL）
#### 7.5 动态链接相关结构
##### 7.5.1 “.interp”段
##### 7.5.2 “.dynamic”段
##### 7.5.3 动态符号表
##### 7.5.4 动态链接重定位符
##### 7.5.5 动态链接时进程堆栈初始化
#### 7.6 动态链接的步骤和实现
##### 7.6.1 动态连接器自举
##### 7.6.2 装载动态对象
##### 7.6.3 重定位和初始化
##### 7.6.4 Linux动态连接器实现
#### 7.7 显式运行时链接
##### 7.7.1 dlopen()
##### 7.7.2 dlsym()
##### 7.7.3 dlerror()
##### 7.7.4 dlclose()
##### 7.7.5 运行时装载的演示程序
#### 7.8 本章小结
### 第8章 Linux共享库的组织
#### 8.1 共享库版本
##### 8.1.1 共享库兼容性
##### 8.1.2 共享库版本命名
##### 8.1.3 SO-NAME
#### 8.2 符号版本
##### 8.2.1 基于符号的版本机制
##### 8.2.2 Solaris中的符号版本机制
##### 8.2.3 Linux中的符号版本
#### 8.3 共享库系统路径
#### 8.4 共享库查找过程
#### 8.5 环境变量
#### 8.6 共享库的创建和安装
##### 8.6.1 共享库的创建
##### 8.6.2 清除符号信息
##### 8.6.3 共享库的安装
##### 8.6.4 共享库构造和析构函数
##### 8.6.5 共享库脚本
#### 8.7 本章小结
### 第9章 Windows下的动态链接
#### 9.1 DLL简介
##### 9.1.1 进程地址空间和内存管理
##### 9.1.2 基地址和RVA
##### 9.1.3 DLL共享数据段
##### 9.1.4 DLL的简单例子
##### 9.1.5 创建DLL
##### 9.1.6 使用DLL
##### 9.1.7 使用模块定义文件
##### 9.1.8 DLL显式运行时链接
#### 9.2 符号导出导入表
##### 9.2.1 导出表
##### 9.2.2 EXP文件
##### 9.2.3 导出重定向
##### 9.2.4 导入表
##### 9.2.5 导入函数的调用
#### 9.3 DLL优化
##### 9.3.1 重定基地址（Rebasing）
##### 9.3.2 序号
##### 9.3.3 导入函数绑定
#### 9.4 C++与动态链接
#### 9.5 DLL HELL
#### 9.6 本章小结
## 第4部分 库与运行库
### 第10章 内存
#### 10.1 内存
#### 10.2 栈与调用惯例
##### 10.2.1 什么是栈
##### 10.2.2 调用惯例
##### 10.2.3 函数返回值传递
#### 10.3 堆与内存管理
##### 10.3.1 什么是堆
##### 10.3.2 Linux进程堆惯例
##### 10.3.3 Windows进程堆管理
##### 10.3.4 堆分配算法
#### 10.4 本章小结
### 第11章 运行库
#### 11.1 入口函数和程序初始化
##### 11.1.1 程序从main开始吗
##### 11.1.2 入口函数如何实现
##### 11.1.3 运行库与I/O
##### 11.1.4 MSVC CRT的入口函数初始化
#### 11.2 C/C++运行库
##### 11.2.1 C语言运行库
##### 11.2.2 C语言标准库
##### 11.2.3 glibc和MSVC CRT
#### 11.3 运行库与多线程
##### 11.3.1 CRT的多线程困扰
##### 11.3.2 CRT改进
##### 11.3.3 线程局部存储实现
#### 11.4 C++全局构造与析构
##### 11.4.1 glibc全局构造与析构
##### 11.4.2 MSVC CRT的全局构造和析构
#### 11.5 fread实现
##### 11.5.1 缓冲
##### 11.5.2 fread_s
##### 11.5.3 fread_nolock_s
##### 11.5.4 _read
##### 11.5.5 文本换行
##### 11.5.6 fread回顾
#### 11.6 本章小结
### 第12章 系统调用与API
#### 12.1 系统调用介绍
##### 12.1.1 什么是系统调用
##### 12.1.2 Linux系统调用
##### 12.1.3 系统调用的弊端
#### 12.2 系统调用原理
##### 12.2.1 特权级与中断
##### 12.2.2 基于int的Linux的经典系统调用实现
##### 12.2.3 Linux的新型系统调用机制
#### 12.3 Windows API
##### 12.3.1 Windows API概览
##### 12.3.2 为什么要使用Windows API
##### 12.3.3 API与子系统
#### 12.4 本章小结
### 第13章 运行库实现
#### 13.1 C语言运行库
##### 13.1.1 开始
##### 13.1.2 堆的实现
##### 13.1.3 IO与文件操作
##### 13.1.4 字符串相关操作
##### 13.1.5 格式化字符串
#### 13.2 如何使用Mini CRT
#### 13.3 C++运行库实现
##### 13.3.1 new与delete
##### 13.3.2 C++全局构造与析构
##### 13.3.3 atexit实现
##### 13.3.4 入口函数修改
##### 13.3.5 stream与string
#### 13.4 如何使用Mini CRT++
#### 13.5 本章小结





