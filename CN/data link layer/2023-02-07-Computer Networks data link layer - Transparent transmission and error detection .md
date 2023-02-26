---
layout: post
title: 计算机网络数据链路层透明传输和差错检测
categories: [计算机网络]
description: 如何实现数据链路层透明传输和差错检测
keywords: computer networks data link layer 
---

## 如何实现对帧的封装和透明传输

1. 封装：自顶向下逐层封装

![image-20230207165201972](https://wendaocsmaster.github.io/images/blog/image-20230207165201972.png)

2. 帧格式——帧的首部和尾部含有重要的控制信息

   + 点对点协议 PPP 链路的帧格式

     ![image-20230207165502224](https://wendaocsmaster.github.io/images/blog/image-20230207165502224.png)

     PPP 帧的首部和尾部的作用之一就是帧定界

   + 传统以太网MAC帧格式

     ![image-20230207165411719](https://wendaocsmaster.github.io/images/blog/image-20230207165411719.png)

     ​		以太网帧格式中并没有定界符，而是在发送以太网帧时在物理层添加 8 字节的前导码，其中前 7B 为前同步码用于接收方进行时钟同步，还有 1 字节的帧定界符， 在发送完一个帧后要间隔一定时间才能发送下一帧，同时也能留给接收方一定的时间准备接收下一帧。这一段时间叫做帧间间隔，经典（传统）以太网的速度为10Mbps，规定发送96bit的时间作为帧间间隔，即9.6us。

   + 为了提高数据链路层传输数据的效率，应当使帧的数据载荷长度尽可能地大于首部和尾部的长度，但是开率到缓存空间的需求以及差错控制等诸多因素，规定帧的数据载荷的长度上限为 1500B，即以太网<font color = red>最大传输单元 MTU</font>

3. 透明传输

   ​		对于 PPP帧，帧收尾各有 1B 的标志作为帧的开始和结束标志，但是当数据载荷部分也有和结束标志一样的 1 字节内容时，如果不做处理会导致接收方错误认为该帧已经传输完毕。由此引出了针对同步传输链路的比特填充法：5110，以及针对异步传输链路的字节填充法（反转义）。

## 差错检测

+ 可检奇数位错误的奇偶校验码

+ 二维奇偶校验

+ 分组奇偶检验的海明码

  [计算机中编码的检错和纠错 — 问道 (wendaocsmaster.github.io)](https://wendaocsmaster.github.io/2023/02/06/Data-error-checking-and-error-correction-in-computer-organization-principles-and-computer-networks/)

+ CRC循环冗余码

  **CRC循环冗余**：除数补（被除数位数-1即生成多项式最高次数）个0除以被除数，做异或运算，最后得余数为FCS，传输原除数补FCS，接收端收到以后除以原被除数，结果为0无错

  例如：

  被除数101001，除数1101

  除数位数为4，则被除数末尾补3个0为101001000

  101001000 / 1101 = 商+余数（FCS）001

  则最终输出的数据为101001001

  除数是数据链路层协商

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
