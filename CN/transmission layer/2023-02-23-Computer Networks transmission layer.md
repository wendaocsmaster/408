---
layout: post
title: 计算机网络传输层
categories: [Blog, 计算机网络]
description: 传输层
keywords: computer networks network layer
---

## 传输层

​		从物理层到数据链路层到网络层分别解决了bit信号的编码传输、数据在一个网络内的传输以及主机在多个网络之间的通信，但是一台主机中可能会有多个正在通信的服务或者进程，通信双方为与相同层次上的通信进程互为对等实体，如何标识这些进程，将进程发送的数据正确传输给对等实体，为运行在主机上的应用进程提供直接的逻辑通信服务，因此传输层协议又叫做端到端协议。

## 具体问题

+ 如何标识位于主机和服务器上的对等实体，即用于通信的进程？端口号
+ 如何实现传输层的可靠传输？[流量控制和拥塞控制](https://wendaocsmaster.github.io/2023/02/25/Computer-Networks-transmission-layer-Flow-control-and-congestion-control/)
+ 如何对传输层报文进行封装？[TCP、UDP报文格式](https://wendaocsmaster.github.io/2023/02/24/Computer-Networks-transmission-layer-TCP/)



## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
