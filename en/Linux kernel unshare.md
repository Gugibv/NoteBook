Most legacy operating system kernels support an abstraction of threads as multiple execution contexts within a process. 

很多老旧的操作系统内核以这样的方式来支持线程：它们把线程抽象成了同一进程内的多个执行上下文。

These kernels provide special resources and mechanisms to maintain these “threads”. 

这些系统内核提供了特殊的资源和管理机制来支持这类线程。

The Linux kernel, in a clever and simple manner, does not make distinction between processes and “threads”. 

而Linux内核，则采用了一种聪明而且简单的方式——在内核层面上，它并不区分进程和线程。

The kernel allows processes to share resources and thus they can achieve legacy “threads” behavior without requiring additional data structures and mechanisms in the kernel. 

Linux内核允许多条进程共享资源。这样做的好处在于，即使内核不再提供额外的数据结构和管理机制，和传统线程类似的行为也能被实现出来。

The power of implementing threads in this manner comes not only from its simplicity but also from allowing application programmers to work outside the confinement of all-or-nothing shared resources of legacy threads. 

以这样的方式实现线程的好处，不仅在于它的简洁性，还在于它使得程序员可以摆脱这个限制去工作：在旧有的线程模型中，共享资源时，要么就是全局共享，要么就不能共享

On Linux, at the time of thread creation using the clone system call, applications can selectively choose which resources to share between threads. 

在Linux中，程序在使用clone这个系统指令去创建线程时，可以选择性地指定哪部分资源会被其他线程所共享，而无需将所有资源都共享出去。

unshare() system call adds a primitive to the Linux thread model that allows threads to selectively ‘unshare’ any resources that were being shared at the time of their creation.

 unshare这个系统指令，为Linux的线程模型增加了一个基本功能：即便一些资源在线程创建时被设置为可共享了，也可以使用unshare指令将这些资源重新收归私有。

OSI and TCP/IP are the most used models to abstract the computer network. 

OSI和TCP/IP是在抽象计算机网络时最常用的两种模型。

OSI, the theoretically better abstraction of the network model, failed to be widely adopted due to its complexity. 

OSI，这种在理论上看似更优的网络模型的抽象方式，由于过于复杂而没有被工业界大规模采用。

```
legacy /ˈlɛɡəsi/ 遗产
kernel /ˈkɜ:rnl/ 核，核心
abstraction /æbˈstrækʃən/ 抽象
thread /θrɛd/ 线程
multiple ['mʌltɪpl] adj. 多重的；多样的；许多的
execution context 执行环境
process /ˈproʊses/ 进程
resource /ˈri:sɔ:rs/ 资源
mechanism /ˈmɛkəˌnɪzəm/ 机制
maintain /menˈten/ 维持，继续
in a manner 以一种什么的方式，或者以一种什么样的方法
distinction /dɪˈstɪŋkʃən/  不同
make distinction between A and B  把A和B区别开来
allow /əˈlaʊ/允许
share resource 共享资源
achieve legacy “threads” behavior  实现和传统的线程模型相似的行为
require /rɪˈkwaɪr/  需要
additional /ə'dɪʃənl/ 额外的
power /ˈpaʊɚ/ 力量，威力
simplicity /sɪmˈplɪsɪti/ 简单
confinement /kənˈfaɪnmənt/ 限制
outside the confinement  摆脱某种限制
implement /ˈɪmpləmənt/ 执行
all-or-nothing 要么就没有，要么就全都要
at the time of 在某一时刻
selectively /sɪ'lektɪvlɪ/  选择性地
unshare 不共享、撤回共享
primitive /ˈprɪmɪtɪv/ 原始的，原语
theoretically /ˌθiəˈretɪkli/ 理论上
adopt /əˈdɑ:pt/ 采用
complexity /kəmˈplɛksɪti/ 复杂性
```