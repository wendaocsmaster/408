---
layout: post
title: 计算机网络网络层
categories: [Blog, 计算机网络]
description: 网络层
keywords: computer networks network layer
---

## 网络层要解决什么样的问题

+ 如何实现各网络之间的主机标识？（网络和主机编址，IP地址）
  + [IPv4编址](https://wendaocsmaster.github.io/2023/02/13/Computer-Networks-network-layer-How-to-implement-the-identification-of-each-host-at-the-network-layer-ipv4/)
  + [IPv6编址](https://wendaocsmaster.github.io/2023/02/13/Computer-Networks-network-layer-How-to-implement-the-identification-of-each-host-at-the-network-layer-ipv6/)
  + [如何规划IPv4地址的分配](https://wendaocsmaster.github.io/2023/02/15/Computer-Networks-network-layer-How-to-plan-ipv4-address-assignment/)
  
+ 如何实现对传输层TCP报文段或者UDP用户数据报的封装？（IPv4、IPv6公网地址和私网地址）

  + [IPv4](https://wendaocsmaster.github.io/2023/02/15/Computer-Networks-network-layer-How-to-implement-encapsulation-of-IPv4-datagrams/)
  + [IPv6](https://wendaocsmaster.github.io/2023/02/15/Computer-Networks-network-layer-How-to-implement-encapsulation-of-IPv6-datagrams/)

+ 如何实现路由器对数据分组的正确转发？（地址解析协议：ARP； 内部网关协议IGP：RIP、OSPF ；外部网关协议：BGP；网际控制报文协议：ICMP）

  + [IP数据报转发的过程](https://wendaocsmaster.github.io/2023/02/14/Computer-Networks-network-layer-The-process-of-sending-and-forwarding-IP-datagrams/)

  + [ARP地址解析协议](https://wendaocsmaster.github.io/2023/02/15/Computer-Networks-network-layer-ARP/)

  + [路由配置](https://wendaocsmaster.github.io/2023/02/16/Computer-Networks-network-layer-Router-configuration/)

  + [路由选择协议RIP](https://wendaocsmaster.github.io/2023/02/15/Computer-Networks-network-layer-RIP/)

  + [路由选择协议OSPF](https://wendaocsmaster.github.io/2023/02/18/Computer-Networks-network-layer-OSPF/)

  + [路由选择协议BGP](https://wendaocsmaster.github.io/2023/02/18/Computer-Networks-network-layer-BGP/)

  + [网际控制报文协议ICMP](https://wendaocsmaster.github.io/2023/02/19/Computer-Networks-network-layer-ICMP/)

+ 如何解决内网访问公网数据？（NAT，NAPT）

  [计算机网络网络层如何解决内网访问公网数据 ](https://wendaocsmaster.github.io/2023/02/15/Computer-Networks-network-layer-How-to-resolve-intranet-access-to-public-network-data/)

+ 如何在公网上传输单位内部数据？（虚拟专用网VPN）

  [计算机网络网络层如何解决内网访问公网数据 ](https://wendaocsmaster.github.io/2023/02/15/Computer-Networks-network-layer-How-to-resolve-intranet-access-to-public-network-data/)

+ 如何实现在网络上“一对多”的通信以节省网络资源？[（IP多播、网际组管理协议：IGMP）](https://wendaocsmaster.github.io/2023/02/22/Computer-Networks-network-layer-IGMP/)

+ 如何实现用户主机在移动过程中保持访问网络资源的连续性？[（移动IP技术）](https://wendaocsmaster.github.io/2023/02/19/Computer-Networks-network-layer-Mobile-IP-Technologies/)

+ [未来网络体系结构SDN](https://wendaocsmaster.github.io/2023/02/22/Computer-Networks-network-layer-SDN/)

​		综上，网络层要解决的问题实际上是如何实现各网络之间数据分组的正确传输。网络层设备路由器的主要功能就是实现数据分组的路由选择和分组转发。网络层可以向上层提供两种服务：

+ 面向连接的虚电路服务：虚电路并不是一条专用的物理链路，而是一条逻辑上的链接，这种服务的核心思想是可靠通信由网络自身来保证。在通信之前，通信双方必须建立<font color =red>网络层连接</font>（这种连接与电路服务的连接并不完全一致），一旦建立连接，通信期间的所有数据分组都沿着固定的路由转发分组，通信结束之后还需要释放连接。采用这种通信方式再使用可靠传输的网络协议，就可以使得数据分组最终正确（无差错按序、不丢失、不重复）到达接收方。

+ 面向无连接的数据报服务：其核心思想是可靠通信由用户主机来保证，将复杂的网络处理功能置于因特网边缘（用户主机及其内部的传输层及其上层），将相对简单的尽最大努力的分组转发交付置于因特网核心（路由器），通信的双方事先不需要建立<font color =red>网络层连接</font>，在进行数据分组的路由转发时候，各数据分组到达目的主机的路径可能不一致。计算机网络体系结构中的网络层就是选择了面向无连接的尽最大努力交付的不可靠服务。

  | 对比         | 虚电路服务                                         | 数据报服务                                                   |
  | ------------ | -------------------------------------------------- | ------------------------------------------------------------ |
  | 核心思想     | 可靠通信由网络自身保证                             | 可靠通信由用户主机保证                                       |
  | 网络层连接   | 必须建立                                           | 不需要建立                                                   |
  | 目的地址     | 仅在建立连接阶段使用，之后每隔分组使用短的虚电路号 | 每个数据分组必须携带完整的网络目的地址                       |
  | 分组转发     | 所有数据分组的路径一致                             | 各数据分组到达目的主机的路径可能不一致                       |
  | 节点故障     | 位于虚电路上的单个节点故障，虚电路服务中断         | 单个节点故障可能会导致某些数据分组丢失，路由器会选择新的转发路径 |
  | 分组顺序     | 按序到达                                           | 不一定按序到达                                               |
  | 服务是否可靠 | 可靠                                               | 不可靠                                                       |



## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
