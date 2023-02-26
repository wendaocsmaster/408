---
layout: post
title: 计算机网络传输层释放连接
categories: [计算机网络]
description: 传输层
keywords: computer networks network layer 
---

## 释放连接

通信双方在通信结束之后要释放TCP连接

1. 客户端发送一个释放连接请求，TCP状态变为FIN-WAIT-1，FIN置为1

   ![image-20230225100434719](https://wendaocsmaster.github.io/images/blog/image-20230225100434719.png)

2. 服务器对于该请求返回确认释放报文，同时将自己的TCP状态转入CLOSE-WAIT![image-20230225100614902](https://wendaocsmaster.github.io/images/blog/image-20230225100614902.png)

3. 客户端在接收到该确认后TCP状态转入FIN-WAIT-2，并等待服务器发回的断开连接请求，此时TCP连接处于半关闭的状态，客户端不能向服务器发送数据但是仍然可以接收来自服务器的数据。

   ![image-20230225101258197](https://wendaocsmaster.github.io/images/blog/image-20230225101258197.png)

4. 半关闭状态可能持续一段时间，服务器向客户端发送请求关闭连接，同时将自己的TCP状态转入LAST-ACK

   ![image-20230225100957274](https://wendaocsmaster.github.io/images/blog/image-20230225100957274.png)

5. 客户端在接收到服务器发送的断开连接请求后，对该请求发回一个确认，同时TCP状态转入TIME-WAIT，并等待2MSL时间后进入CLOSED状态，服务器在接收到该确认后进入CLOSED状态

   ![image-20230225101806518](https://wendaocsmaster.github.io/images/blog/image-20230225101806518.png)

   <font color = red>客户端对服务器的断开连接发送确认后还需要等待2MSL时间的原因</font>

   **<font color = red>如果主机在发送确认后立即进入CLOSED状态，而此时很不幸客户端发送的确认报文在网络中丢失了，服务器收不到该确认，当服务器的请求断开连接报文的超时重传计时器倒计时结束，服务器会继续重传该请求报文，但是此时客户端已经无法对该报文做出响应，导致一段时间内该TCP连接无法关闭，浪费服务器主机资源和网络资源</font>**

6. TCP保活计时器

   ![image-20230225102622894](https://wendaocsmaster.github.io/images/blog/image-20230225102622894.png)

## 参考资料

[网络是怎样连接的](https://book.douban.com/subject/26941639/)(日)*户根勤著*; *周自恒译*. 责任者, 户根勤; 周自恒. 出版发行, 北京: 人民邮电出版社,2017

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
