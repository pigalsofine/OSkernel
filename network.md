# lwip

- 简介

> lwip(Light weight internet protocol)是瑞典计算机学院开发的一套用于嵌入式系统的开源TCP/IP协议栈

- 特点

> 轻型IP协议，实现重点是在保持TCP协议的主要功能的基础上减少对RAM的占用。适合小型嵌入式系统

> 是一种相对松散的通讯机制，通过共享内存的方式实现应用层与底层协议族之间的通讯。尽量避免了内存复制，避免了内粗复制产生的性能损失

- 文件组织

> |---- lwip

>> |---- src

>>> |---- api (应用程序接口文件)

>>> |---- arch （与硬件和OS有关的文件，包括网络驱动，移植需要修改的文件，“自己创建目录”）

>>> |---- core （LWIP的核心代码，包括ICMP,IP,UDP,TCP等协议的实现）

>>> |---- include （LWIP的包含文件）

>>> |----netif （提供了网络接口驱动程序的基本框架）


- API的实现

> API被分成两部分实现，一部分作为应用程序的链接库实现，另一部分在TCP/IP进程内部实现，这两部分之间采用由操作系统模拟层提供的进程间通讯机制(IPC)进行通讯

> API 函数库中处理网络连接的函数驻留在TCP/IP 进程中，位于应用程序进程中的API 函数使用一个简单的通讯协议向驻留在 TCP/IP 进程中的API 函数传递消息，这个消息包括应该执行的操作数及操作参数，驻留在TCP/IP 进程中的API函数执行这个操作并通过消息机制向应用程序返回操作结果

# libuinet

> 将 FreeBSD 9.x 的 TCP/IP 协议栈移植到了用户态。

> 在用户态运行 TCP/IP 协议栈意味着并发 TCP 连接不再占用系统文件数，只占内存，解决了 C1000k 的一大瓶颈，内核只要提供一个收发网络 packet 的接口就行

-  优点

> 1.来源于BSD协议栈，功能支持全面，所以socket,ipv4,ipv6都支持

> 2.来源于BSD协议栈，稳定性和社区活跃性都不错

> 3.bsd协议栈提供接口与linux socket编程接口兼容性好

- 缺点

> 1.代码太复杂，因为是从bsd代码中porting过来的，所以有大量对我们来说无用的代码

> 2.不支持DPDK。而且二三四层绑定比较紧密~想拆出来不太容易

> 3.也是线程模型~与MTCP有同样的缺点

> 4.除了协议栈还有大量中间代码（例如协议栈创建了4个线程，而且这个线程不是用标准的pthread创建，也就是它自己还带了一套线程库）

> 5.性能没有测试数据，以及内存占用等都没有对应的数据~而且如果一旦出现性能问题~因为代码过与复杂，优化上风险比较大；

- 总结

> bsd用户态协议栈功能强大，社区活跃，但是复杂度太高，而且没有现成的与dpdk对接的例子，移植成本比较高，性能上没有准确数据~如果有性能问题，优化难度较大

# lkl

>设计目标是在用户态复用linu成熟的功能实现，目前主要有协议栈，文件系统

# TCP/IP

> 协议栈实现机制

> https://blog.csdn.net/yasi_xi/article/details/8089426
