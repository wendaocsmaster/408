---
layout: post
title: 计算机网络传输层TCP
categories: [计算机网络]
description: 传输层
keywords: computer networks network layer
---

## TCP格式

![image-20230225084129711](https://wendaocsmaster.github.io/images/blog/image-20230225084129711.png)

源端口：发送方端口，仅对发送方有意义

目的端口：接收方端口，仅对接收方有意义

序号：标识本TCP报文段中的数据载荷的第一个字节的序号

确认号：指出期望收到的下一个TCP报文段的第一个字节的序号，以及说明本确认号之前的所有数据均正确接收

ACK：确认标志位，TCP连接正确建立之后都应设置ACK为1

RST：重置连接，连接出错时候使用还可以用来拒绝非法报文段和拒绝打开TCP连接

SYN：TCP建立连接时候使用

FIN：释放连接时候使用

PSH：减小报文段被提交给上层的等待时间，一旦传输层接收到PSH为1的TCP报文段立即向上层提交，而不是等待到达一批后再向上层一次性提交

URG：1时紧急指针字段有效0时候无效，紧急指针指明数据的长度

数据偏移字段：以4B为单位，表示数据载荷部分距离TCP报文段的开始字节的长度，即TCP报文首部长度0101-1111

窗口字段：以1B为单位，指明接收方和发送方的窗口大小

检验和：二进制反码计算，与网络层的首部校验方式不同在于TCP首部校验时候还需要加上12B的伪首部

![image-20230225085707328](https://wendaocsmaster.github.io/images/blog/image-20230225085707328.png)



## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
