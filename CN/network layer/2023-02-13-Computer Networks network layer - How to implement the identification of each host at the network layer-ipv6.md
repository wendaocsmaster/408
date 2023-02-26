---
layout: post
title: 计算机网络网络层如何实现网络层各主机的标识——IPv6
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer
---

## IPv6编址

1. IPv6地址空间大小为$$2^{128}$$

2. IPv6地址的表示方法

   + 冒号十六进制表示法

     ![image-20230214231002960](https://wendaocsmaster.github.io/images/blog/image-20230214231002960.png)

     “左侧零”省略和“连续零”压缩

     + “左侧零”省略是指两个冒号中间的十六进制数中最前面的一串零可以省略不写
     + “连续零”压缩是指连续的一连串零可以用一对冒号替代

     ![image-20230214231115537](https://wendaocsmaster.github.io/images/blog/image-20230214231115537.png)

     ![image-20230214231226161](https://wendaocsmaster.github.io/images/blog/image-20230214231226161.png)
     
   + 冒号十六进制结合点分十进制：在IPv4向IPv6过渡阶段非常有用

     ![image-20230214231647360](https://wendaocsmaster.github.io/images/blog/image-20230214231647360.png)

   + CIDR斜线表示法

     ![image-20230214231745462](https://wendaocsmaster.github.io/images/blog/image-20230214231745462.png)

3. IPv6地址的分类

   + 数据数据报的目的地址有三种基本类型

     + 单播地址：传统点对点的通信
     + 多播地址：一对多通信，IPv6将广播看做是多播的一个特例
     + 任播地址：IPv6新增的一种类型，任播的终点是一组计算机，但是数据报只交付其中的一个，通常是按照路由算法得出的最近的一个。

   + 未指明地址：128bit全0的地址，缩写为"::"。该地址不能用于目的地址，只能用于还没有配置到一个标准IPv6的主机作为源地址

   + 环回地址：最低比特为1，其余127比特全为0，计为0:0:0:0:0:0:0:1，可缩写为"::1"。可以作为源地址和目的地址

   + 多播地址：最高8bit全为1的地址，可以计为"FF00::/8"占所有IPv6地址的比例为$$2^{128-8}/2^{128}=1/256$$

   + 本地链路单播地址最高10bit为1111 1110 10的地址，可记为fe80::/10。这类地址占IPv6地址总空间的$$1/1024$$，连接在这种网络上的主机都可以使用本地链路单播地址进行通信，但是不能和因特网上的其他主机通信。类似IPv4中的私网地址。

   + 全球单播地址：IPv6全球单播地址使用三级结构，方便路由器更快查找路由

     前48bit分配给公司和机构（相当于IPv4分类地址中的网络号），中间16bit用于各单位构建自己的子网，最后64bit用于指明主机或者路由器的各个接口，相当于IPv4分类地址中的主机号
     
     ![image-20230214234507780](https://wendaocsmaster.github.io/images/blog/image-20230214234507780.png)

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
