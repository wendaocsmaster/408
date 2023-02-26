---
layout: post
title: 计算机网络网络层IP数据报转发过程
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer
---

## IP数据报的发送和转发转发

### 主机发送IP数据报

1. 直接交付和间接交付

   ![image-20230215000253628](https://wendaocsmaster.github.io/images/blog/image-20230215000253628.png)

   + 直接交付：当通信双方主机在同一个网络内时候直接交付
   + 间接交付：当通信双方主机不在同一个网络内时候交给路由器转发间接交付

2. 如何判断目标主机与源主机是否在同一个网络中

   ![image-20230215000308746](https://wendaocsmaster.github.io/images/blog/image-20230215000308746.png)

   ​		将目的主机的IP地址与自己的IP地址的地址掩码进行逻辑与运算，将自己的IP地址和地址掩码进行逻辑与运算，两个结果比较如果相同就是在同一个网络，如果不同就是在不同的网络

3. 如何选择交给哪个路由器来转发IP数据报

   ![image-20230215000358295](https://wendaocsmaster.github.io/images/blog/image-20230215000358295.png)

   通常会设置一个默认路由，也叫作默认网关。

### 路由器转发IP数据报

对于通过路由器转发的IPv4数据报，路由器在收到数据报后通常进行以下两个步骤：

+ 检查收到的IP数据报是否正确，首部是否误码，生存时间TTL是否为0，如果不正确就丢弃该IP数据报，并立即向源主机发送该IP数据报的的差错报告报文
+ 查表转发（查的是转发表，是根据路由表优化搜索而来的），如果找到匹配的路由条目则转发，否则丢弃该IP数据报并立即向该IP数据报的源主机发送差错报告报文

以单播报文为例：

![image-20230215001508980](https://wendaocsmaster.github.io/images/blog/image-20230215001508980.png)

以广播报文为例：路由器默认不转发广播报文

![image-20230215001606024](https://wendaocsmaster.github.io/images/blog/image-20230215001606024.png)

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
