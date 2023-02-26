---
layout: post
title: 计算机网络网络层路由选择协议BGP
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## 路由选择协议BGP

1. 由于在不同的自治系统内衡量路由的代价是不同的，而且自治系统AS之间的路由选择还会涉及到政治、经济、安全、网络运营商等，所以在自制系统之间的寻找最佳路由是不现实的。

2. 使用BGP边界网关协议需要设置BGP边界路由器

   + 每个自治系统的管理员至少要配置一个路由器作为该自治系统的BGP发言人
   + 一般来说，两个BGP发言人是通过一个共享网络连接在一起的，而BGP发言人往往就是BGP路由器
   + 使用TCP连接交换路由信息的两个BGP发言人，彼此称为对方的邻站或者对等站
   + BGP发言人除了要运行BGP协议外，还必须运行自己所在的自制系统所使用的RIP或者OSPF
   + BGP发言人交换网络可达性的信息，就是要到达某个网络索要经过的一系列自治系统。
   + 当BGP发言人交换好了网络可达性信息后，各BGP发言人就根据所采用的策略，从收到的路由信息中找出到达各自治系统的较好的路由，也就是构造出树形结构且不存在环路的自治系统连通图

   ![image-20230221195025714](https://wendaocsmaster.github.io/images/blog/image-20230221195025714.png)

   ![image-20230221195224034](https://wendaocsmaster.github.io/images/blog/image-20230221195224034.png)

3. BGP-4是目前使用最多的版本，共有四种报文

   + 打开报文：与邻站建立连接
   + 保活报文：周期性验与邻站的连通性
   + 更新报文：通告某一条路由信息，以及列出要求撤销的多条路由
   + 通知报文：发信检测到的差错

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
