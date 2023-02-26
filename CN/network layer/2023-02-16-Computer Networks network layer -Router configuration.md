---
layout: post
title: 计算机网络网络层路由配置
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## 路由配置

1. 直连路由和非直连路由

   + 直连路由：当管理员对路由器的各接口进行IP地址和地址掩码配置后，路由器就可以自行得出自己的各接口分别与哪些网络直连，中间没有其他路由器

   + 非直连路由：路由器到其他非直连网络的路由需要通过路由选择协议进行自动配置，也可以通过管理员配置。

     ![image-20230216101450297](https://wendaocsmaster.github.io/images/blog/image-20230216101450297.png)

2. 默认路由和特定主机路由

   + 配置默认路由后，当数据报中的目的IP地址与路由器转发表中的目的网络没有匹配后，路由器可以将该IP数据报转发给默认路由器。配置目的网络地址0.0.0.0，地址掩码0.0.0.0，记为0.0.0.0/0，如下图IP地址为192.168.1.254/24的主机想要访问因特网中的资源，该主机发出的请求IP数据报到达路由器R1，路由器R1将该IP数据报的IP地址和自己转发表中的目的网络进行匹配，发现没有匹配项，然后将该IP数据报转发给默认路由R2，R2收到后再进行转发。

     ![image-20230216102046030](https://wendaocsmaster.github.io/images/blog/image-20230216102046030.png)

   + 特定主机路由，路由表中的目的网络为特定主机的IP地址，地址掩码为255.255.255.255

     ![image-20230216102522247](https://wendaocsmaster.github.io/images/blog/image-20230216102522247.png)

   + 路由器在进行查表转发时候，遵循“最长前缀匹配”的原则，特定主机路由条目的匹配优先级最高，默认路由的匹配优先级最低，因为能够匹配的网络前缀越多，网络就越具体，查找目的主机的范围就越小。
   + 在进行网络配置时候如果路由条目配置错误，有可能导致网络出现环路，聚合路由时候[可能引入不存在的网络](https://wendaocsmaster.github.io/2023/02/13/Computer-Networks-network-layer-How-to-implement-the-identification-of-each-host-at-the-network-layer-ipv4/)。

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
