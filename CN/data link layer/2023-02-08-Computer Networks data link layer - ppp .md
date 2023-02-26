---
layout: post
title: 计算机网络数据链路层点对点链路协议封装格式
categories: [计算机网络]
description: 如何实现数据链路层封装
keywords: computer networks data link layer 
---

## PPP协议

1. PPP协议是数据链路层使用最广泛的点对点协议

2. 应用：拨号上网PPPoE和广域网链路路由器之间

3. 组成及其在TCP/IP体系结构中的地位：

   + 组成：链路控制协议（LCP子层）:arrow_right:网络控制协议（NCP子层），LCP子层进行链路身份验证，通过之后由NCP 协商网络地址
   + 上层网络层 IP 族不同的协议以及不同硬件平台的网络层协议数据单元均可由PPP 承载
   + 下层是物理层，有面向比特的同步链路和面向字节的异步链路

4. 帧格式：

   ![image-20230211103640896](https://wendaocsmaster.github.io/images/blog/image-20230211103640896.png)

   + 标志：PPP的帧定界符，用于确定帧的开始和结束

   + 地址：FF，预留目前没用

   + 控制：03，预留目前没用

   + 协议：用来指出帧的数据部分载荷向上交给哪个协议处理

   + FCS：帧检验序列，CRC循环冗余检验

   + 帧的透明传输：

     + 面向比特：5110

     + 面向字节：

     + ![image-20230211104245511](https://wendaocsmaster.github.io/images/blog/image-20230211104245511.png)

       7E:arrow_right:7D 5E

       7D:arrow_right:7D 5D

       03:arrow_right:7D 23

5. PPP协议的工作状态：以用户主机拨号接入因特网服务提供者ISP的拨号服务器的过程为例

   ![image-20230211104515543](https://wendaocsmaster.github.io/images/blog/image-20230211104515543.png)

   



![PPP Protocol](https://wendaocsmaster.github.io/images/blog/ppp-protocol2.png)



## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
