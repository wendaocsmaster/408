---
layout: post
title: 计算机网络数据链路层虚拟局域网
categories: [计算机网络]
description: 如何虚拟局域网分离广播域
keywords: computer networks data link layer 
---

## VLAN

### 概述

1. 将多个站点通过一个或者多个以太网交换机连接起来就形成了交换式以太网，交换式以太网中的所有站点的属于同一个广播域。随着交换式以太网规模的增加，广播域也相应增大。网络中会频繁出现广播信息（ARP、RIP、DHCP等）巨大的广播域有可能遇到广播风暴浪费网络资源和个主机CPU资源，并且难以管理和维护。可以使用路由器隔离广播域，但是成本较高。由此产生了VLAN。

2. 虚拟局域网VLAN将局域网内的各站点划分成与物理位置无关的逻辑组，属于同一vlan的各站点之间可以通信，不同vlan的各站点之间不能直接通信。连接在同一交换机上的多个站点可以属于不同vlan，连接在不同交换机的不同站点可以属于同一vlan。

   ![image-20230212114903083](https://wendaocsmaster.github.io/images/blog/image-20230212114903083.png)

### 实现方式

1. IEEE 802.1Q帧：对以太网V2的帧进行扩展，在源地址字段和类型字段之间插入4B的VLAN标签字段

![image-20230212115110531](https://wendaocsmaster.github.io/images/blog/image-20230212115110531.png)

![image-20230212115228958](https://wendaocsmaster.github.io/images/blog/image-20230212115228958.png)

​		标签协议标识符TPID：长度16bit，其值固定为 0x8100，表明是 IEEE 802.1Q帧

​		优先级PRI：长度为3bit，取值 0-7，值越大优先级越高。当网络阻塞时候设备有限发送优先级高的802.1Q帧

​		规范格式CFI：长度为1bit，取值为0标识MAC地址以规范格式封装，取值为1表示MAC地址以非规范格式封		装，对于以太网，CFI = 0

​		虚拟局域网标识符VID：长度为12bit，取值0-4095，其中0和4095不用，<font color = red>VID是802.1Q帧所需VLAN的编号，</font>		<font color =red>设备利用VID来识别帧所属的VLAN</font>，广播帧只在一个VLAN内转发

2. 交换机对于802.1Q帧的处理

   + 当交换机收到普通的以太网MAC帧时候，就会对其插入4B的VLAN标识符使其成为802.1Q帧，简称“打标签”
   + 当交换机转发802.1Q帧时候可能会删除其4B的VLAN标识符使其成为普通的以太网MAC帧，简称为“去标签转发”。是否要去标签转发，取决于交换机的接口类型。

3. 以太网交换机接口类型：在一个交换机上不进行人为的划分，交换机各个接口属于默认的VLAN1且类型为Access。

   + Access：连接主机，Access接口只能属于一个VLAN，因此接口的PVID值与所属VLAN的ID相同，默认值为1。

     + 默认情况

       <img src="https://wendaocsmaster.github.io/images/blog/image-20230212121219157.png" alt="image-20230212121219157" style="zoom:50%;" />

     + 一个交换机划分两个VLAN

       <img src="https://wendaocsmaster.github.io/images/blog/image-20230212121312834.png" alt="image-20230212121312834" style="zoom:50%;" />

       <img src="https://wendaocsmaster.github.io/images/blog/image-20230212121343922.png" alt="image-20230212121343922" style="zoom:50%;" />

   + Trunk：连接交换机，Trunk端口用于交换机之间的互连。Trunk接口可以通过属于不同VLAN的帧。默认PVID值为1，一般不修改，<font color = red>如果互连的Trunk接口的PVID值不相等，可能出现转发错误。</font>

     + 接收处理：对于未打标签的普通以太网MAC帧，根据接收帧的PVID“打标签”

     + 转发处理：对于已经打标签的帧如果帧的PVID值与接口的PVID值相等则去标签转发，如果不等则直接转发

     + 帧的PVID和Trunk接口的PVID值相等去标签转发

       <img src="https://wendaocsmaster.github.io/images/blog/image-20230212122159795.png" alt="image-20230212122159795" style="zoom:50%;" />

     + 帧的PVID和Trunk接口的PVID值不等直接转发

       <img src="https://wendaocsmaster.github.io/images/blog/image-20230212122247407.png" alt="image-20230212122247407" style="zoom:50%;" />

   + Hybrid（华为独有）：既可以用于交换机与用户计算机1之间的互连（Access）也可以用于交换机之间的互连，但是在转发时候，Hybrid接口查看帧的PVID值是否在去标签列表中，如果在则去标签转发，否则直接转发。

### Vlan案例

![image-20230121100454500](https://wendaocsmaster.github.io/images/blog/image-20230121100454500.png)

1.1 主机A发送广播帧，交换机1对广播帧打标签，由于端口1的PVID = 1，所以VID = 1，交换机2端口对于广播帧去标签转发到主机B，Trunk端口5的PVID = 1，也对广播帧进行去标签转发到交换机2，交换机2的端口5的PVID = 2，对于受到的广播帧打标签VID = 2，交换机2的端口3和4的PVID = 2，交换机2对于广播帧去标签转发到交换机2的端口3和4。

综上位于vlan1的主机A发送的广播帧正确发送到了主机B，错误发送到了主机G和主机H

1.2 主机C发送广播帧，由于主机C的PVID = 2，所 以交换机对于主机C发送的广播帧打标签VID = 2，主机D的PVID = 2，且和主机C位于同一交换机，交换机1对于广播帧去标签转发到端口4，广播帧正确发送到主机D。交换机1端口为Trunk端口，且PVID = 1，所以对于PVID = 2的广播帧直接转发进入交换机2，交换机2的端口5为Trunk端口，交换机2的端口3和端口4的PVID = 2，交换机2对于该广播帧进行去标签转发到端口3和端口4，主机G和主机H正确接收到主机C发送的广播帧。

综上，位于vlan2的主机C发送的广播帧正确转发到了主机D，主机G和主机H。

2.1 主机A发送的广播帧被正确发送到主机B、主机E和主机F

2.2 主机C发送的广播帧被正确发送到主机主机D，错误发送到主机E和主机F。

3.1 主机A发送的广播帧被正确发送到主机B、主机E和主机F。

3.2 主机C发送的广播帧被正确发送到主机D、主机G和主机H。

4.1 主机A发送的广播帧被正确发送到主机B、主机E和主机F。

4.2 主机C发送的广播帧被正确发送到主机D、主机G和 主机H。

综上，互联的Trunk端口的PVID值不相等，可能造成错误转发

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
