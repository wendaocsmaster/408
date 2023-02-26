---
layout: post
title: 计算机网络传输层协议
categories: [计算机网络]
description: 传输层
keywords: computer networks network layer
---

## 传输层

​		从物理层到数据链路层到网络层分别解决了bit信号的编码传输、数据在一个网络内的传输以及主机在多个网络之间的通信，但是一台主机中可能会有多个正在通信的服务或者进程，通信双方为与相同层次上的通信进程互为对等实体，如何标识这些进程，将进程发送的数据正确传输给对等实体，为运行在主机上的应用进程提供直接的逻辑通信服务，因此传输层协议又叫做端到端协议。

![image-20230225081515313](https://wendaocsmaster.github.io/images/blog/image-20230225081515313.png)

## 传输层提供的服务以及对应协议

传输层可以向上层提供面向连接的可靠的服务，使用TCP（Transmission Control Protocol）协议即传输控制协议

传输层可以向上层提供无连接的不可靠的服务，使用UDP（User Datagram Protocol）协议即用户数据报协议

![image-20230225081942783](https://wendaocsmaster.github.io/images/blog/image-20230225081942783.png)

+ TCP

  通信双方通信之前必须建立逻辑连接（三次握手）、通信结束必须释放连接（四次挥手）

  为了实现可靠传输TCP采用连接管理、确认机制、超时重传、流量控制和拥塞控制

  TCP仅支持一对一的单播通信

  面向字节流

+ UDP

  通信双方不需要提前建立连接也

  不使用可靠传输机制

  UDP支持一对一的单播通信以及一对多的多播通信，以及一对全的广播通信

  面向用户数据报

+ TCP和UDP相比，实现跟我给复杂，TCP首部报文段的首部较大占用处理机的资源较多20-60B，UDP的实现更为简单，UDP用户数据报的首部较小仅8B

## 端口

1. 运行在计算机上的进程使用进程标识符PID来标识，在不同操作系统上的PID又有不同的实现方式，为了使得运行不同操作系统的计算机应用进程能够基于网络通信，传输层使用端口号来标识通信双方的进程。
2. 端口号使用16bit来表示，0-65535，其中0-1023为熟知端口号，1024-49151为登记端口号，客户端端口号一般使用短暂端口号41952-65535，端口号只具有本地意义以，不同计算机中的相同端口号是没有关系的。

![image-20230225083453876](https://wendaocsmaster.github.io/images/blog/image-20230225083453876.png)

## 协议以及端口的复用、分用

![image-20230225083414772](https://wendaocsmaster.github.io/images/blog/image-20230225083414772.png)

![image-20230225083555620](https://wendaocsmaster.github.io/images/blog/image-20230225083555620.png)



## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
