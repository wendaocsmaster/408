---
layout: post
title: 计算机网络传输层建立连接
categories: [计算机网络]
description: 传输层
keywords: computer networks network layer 
---

## 建立连接

TCP协议为上层提供面向连接的可靠的数据传输服务，在通信开始之前要通过三次握手建立连接。三次握手主要用于通信双方协商参数例如最大报文段长度，最大窗口大小，时间戳等以及对通信资源进行分配和初始化。建立连接的过程如下

1. 初始时刻，通信双方并没有建立逻辑连接，TCP状态处于CLOSED，服务器端想要接收客户端的请求，创建传输控制块TCB，TCP状态变为LISTEN

   ![image-20230225093115449](https://wendaocsmaster.github.io/images/blog/image-20230225093115449.png)

2. 客户端主动发送请求连接，SYN同步信号设置为1，为seq设置一个初始值，客户端TCP状态变为SYN-SENT

   ![image-20230225093856708](https://wendaocsmaster.github.io/images/blog/image-20230225093856708.png)

3. 服务器对于TCP请求连接返回确认响应，服务器TCP状态变为SYN-RCVD，SYN=1，为自己的发送序号初始化

   ![image-20230225093916209](https://wendaocsmaster.github.io/images/blog/image-20230225093916209.png)

4. 客户端接收到服务器返回的确认建立连接后TCP状态变为ESTABLISHED并且返回一个普通确认<font color =red>TCP规定对于普通确认如果不携带数据则不消耗序号，对于下图中客户端发送的确认而言，如果不携带数据则下一个数据报文段的序号仍然为 x+1</font>

   ![image-20230225094130937](https://wendaocsmaster.github.io/images/blog/image-20230225094130937.png)

5. 服务器收到该确认后TCP状态变为ESTABLISHED，此时双方可以发送数据

   ![image-20230225094322737](https://wendaocsmaster.github.io/images/blog/image-20230225094322737.png)

6. 三次握手不能调整为两次握手，由于网络环境复杂，客户端发出的建立连接请求可能滞留在网络中导致发送方超时计时器倒计时为0并重新发送请求建立连接报文段，并与服务器顺利结束通信，此时之前滞留在网络中的数据报到达服务器，服务器误认为这是一个新的请求建立连接而重新打开一个TCP连接，并向客户端发回响应，但是此时客户端的TCP连接已经关闭而对该报文不予理睬，服务器在超时重传计时器倒计时结束后会继续发送该报文，这样不仅仅浪费了服务器的主机资源同时也浪费了网络资源。

   ![image-20230225094826421](https://wendaocsmaster.github.io/images/blog/image-20230225094826421.png)

## 参考资料

[网络是怎样连接的](https://book.douban.com/subject/26941639/)(日)*户根勤著*; *周自恒译*. 责任者, 户根勤; 周自恒. 出版发行, 北京: 人民邮电出版社,2017

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
