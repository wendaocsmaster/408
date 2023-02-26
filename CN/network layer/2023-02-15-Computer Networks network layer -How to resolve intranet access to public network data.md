---
layout: post
title: 计算机网络网络层如何解决内网访问公网数据
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## 如何解决内网访问公网数据

+ 虚拟专用网VPN：利用公用的因特网作为本机构各个专用网之间的通信载体。实现物理上分散的各机构专用网之间的安全通信，同时解决在公网访问私网数据的问题。

  ![image-20230215220801660](https://wendaocsmaster.github.io/images/blog/image-20230215220801660.png)

  ![image-20230215220850226](https://wendaocsmaster.github.io/images/blog/image-20230215220850226.png)

  公网中的路由器对目的地址是私网的IP数据报一律不转发

  ![image-20230215221132691](https://wendaocsmaster.github.io/images/blog/image-20230215221132691.png)

  ​		在两个专用网出口的路由器上安装相应的VPN服务器软件并进行一系列设置，当一个专用网内的主机要访问另一个专用网内的资源时候，主机发出的IP数据报到达私网出口路由器，路由器对该数据报进行加密，并将加密后的该IP数据报进行封装，源地址改为本路由器的IP地址，目的地址为要访问的私网入口路由器的IP地址，目的路由器收到该IP数据报后进行解封装并进行解密

  ![image-20230215221714562](https://wendaocsmaster.github.io/images/blog/image-20230215221714562.png)

  解密后的IP数据报的首部地址就是私网地址。

  ![image-20230215221906486](https://wendaocsmaster.github.io/images/blog/image-20230215221906486.png)

  VPN的类型有：

  内联网VPN：同一机构不同部门

  外联网VPN：本机构的虚拟专用网需要某些外部机构参加

  远程接入VPN：在任何地点通过接入互联网访问专用网

+ 网络地址转换NAT及NAPT

  + NAT（Network Address Translation，NAT）：缓解IPv4地址的消耗，能够使得内部专用网中的用户共享少量公网地址来访问因特网。实际上就是在私网出口路由器上进行NAT配置，私网主机访问公网资源时候，对IP数据报中的源地址用NAT的公网地址进行替换，并在路由器中进行记录，当公网服务器的响应返回时候，NAT可以查看该响应IP数据报中的目的地址，该目的地址就是为之前访问该公网资源的私网主机分配的，查找私网主机地址与公网地址分配记录，再次对IP数据报中的目的地址进行替换，替换为访问该资源的私网主机地址。

    私网主机请求访问公网资源过程如下图：

    ![image-20230215223144166](https://wendaocsmaster.github.io/images/blog/image-20230215223144166.png)

    公网服务器发回私网主机对应的请求响应过程如下图：

    ![image-20230215223342826](https://wendaocsmaster.github.io/images/blog/image-20230215223342826.png)

  + 网络地址与端口转换NAPT

    网络地址转换技术NAT虽然缓解了IPv4地址的消耗，但是使用NAT技术能够同时支持访问公网的主机数量有限。由于目前绝大多数网络应用都是使用TCP和UDP运输层协议，因此为了更有效利用NAT路由器中的公网地址，将NAT与运输层端口号结合提出了NAPT

    私网访问公网的过程如下图：

    ![image-20230215224214053](https://wendaocsmaster.github.io/images/blog/image-20230215224214053.png)

    位于私网内的主机A想要访问公网服务器上的资源，主机A发出访问请求到达NAPT路由器，路由器发现该IP数据报的目的地址是公网地址，从该NAPT路由器的公网地址池中分配一个公网IP地址，同时随机分配一个端口（一般是1024以上-65535），将该IP数据报的源地址替换为为其分配的公网地址，将原私网主机的端口号替换为NAPT路由器随机分配的端口号，并对一系列替换进行记录，然后转发给目的主机，目的主机收到该请求后发回对应的响应。

    ![image-20230215224851228](https://wendaocsmaster.github.io/images/blog/image-20230215224851228.png)

    该响应的源地址为公网服务器的地址，源端口为公网服务器的端口，目的地址为该响应IP数据报对应的请求IP数据报中的NAPT路由器的地址，目的端口为该响应IP数据报对应的请求IP数据报中的NAPT路由器的端口。NAPT路由器收到该响应IP数据报后查找记录进行目的地址和目的端口的替换，将目的端口从NAPT路由器的端口替换为发出请求的私网主机A的端口，目的地址替换为A的地址，然后转发给目的主机A。

  + NAT和NAPT技术虽然在很大程度上缓解了IPv4地址资源的消耗，但是使用这两种技术的通信必须首先由私网主机发起，因此位于内网中的主机并不能直接充当因特网中的服务器，需要使用特定的内网穿透技术。

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
