---
layout: post
title: 计算机网络网络层IPv6数据报封装
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## IPv6数据报封装格式

![image-20230215114919752](https://wendaocsmaster.github.io/images/blog/image-20230215114919752.png)

1. 版本：4bit，表示IP协议的版本，IPv6数据报该字段的值为6

2. 通信量类：8bit，用于区分不同的数据报的类别和优先级

3. 流标号：20bi，IPv6提出了流的抽象概念，“流”就是因特网从特定源点到特定重点的一系列IPv6数据报，而在这个“流”所经过的路径上的左右路由器都保证指明的服务质量

   属于同一个流的IPv6数据报有相同的流标号，流标号用于资源分配，流标号对于实时音视频的传输特别有用，对于传统的非实时数据没有用处，流标号置为0.

4. 有效载荷长度：16bit，最大65535字节（首部加数据部分）

5. 下一个首部：相当于IPv4数据报首部中的协议字段或可选字段。当IPv6没有扩展首部时，该字段的值就和IPv4的协议字段一样，指明数载荷部分是何种协议单元，向上提交给哪个协议处理

   当IPv6数据报基本首部后面带有扩展首部时候，该字段的值就是其后第一个扩展首部的类型

6. 跳数限制：8bit，和IPV4中TTL的作用一致。在数据报发送时发送方设定一个最大跳数，防止数据报在有环路的网络中无限循环，浪费网络资源。每经过一个路由器该值就减少1，当减少为0时候就丢弃该数据报并立即向源主机发送网际控制报文ICMP

### IPv6扩展首部

1. IPv4数据报如果在首部中使用了可选字段，则在整个IP数据报传送路径上的所有路由器都要对选项字段进行检查，降低了路由器处理数据报的速度

2. IPv6把不需要检查的可选字段放在了扩展首部中，由通信双方主机来处理，提高了路由器的转发效率。

3. IPv6定义了6种扩展首部。详细查看[RFC2460]((https//www.rfc-editor.org/rfc/rfc2460#page-6)

   ~~~
    A full implementation of IPv6 includes implementation of the
      following extension headers:
   
              Hop-by-Hop Options
              Routing (Type 0)
              Fragment
              Destination Options
              Authentication
              Encapsulating Security Payload
   
      The first four are specified in this document; the last two are
      specified in [RFC-2402] and [RFC-2406], respectively.
   ~~~

### The transition from ipv4 to ipv6

1. 双协议栈，主机或者路由器装有两套协议栈。

   + 当IPv6数据报要进入IPv4网络时，将IPv6数据报的首部替换为IPv4数据报首部，但是可能会损失一部分信息，例如流标号
   + 当IPv4数据报进入IPv6网络时候，将IPv6的数据报首部恢复，但是某些信息可能已经丢失无法恢复，例如流标号等。

   如下图所示使用双协议栈进行通信

   ![image-20230215121128183](https://wendaocsmaster.github.io/images/blog/image-20230215121128183.png)

2. 隧道技术Tunneling

   + 当IPv6数据报进入IPv4网络时，将整个IPv6数据报作为IPv4数据报的数据载荷重新封装。
   + 当被IPv4封装后的IPv6数据报要重新进入IPv6网络时候，再讲IPv4数据报解封装，取出原来的IPv6数据报并转发。

   ![image-20230215121542212](https://wendaocsmaster.github.io/images/blog/image-20230215121542212.png)

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
