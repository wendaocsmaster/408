---
layout: post
title: 计算机网络数据链路层无线局域网封装格式
categories: [计算机网络]
description: 如何实现数据链路层封装
keywords: computer networks data link layer 
---

## 802.11无线局域网CSMA/CA协议

1. 分类：802.11无线局域网可以分为两类，有固定基础设施的和无固定基础设施

2. 有固定基础设施的无线局域网：

   + 基本服务集：AP + 主机

     SSID：服务集标识符，AP无线局域网名字

     本BSS内各站点之间的通信及与本BSS之外的站点之间的通信必须经过本BSS内的AP转发

3. 802.11定义的一些基本服务：

   + 主动扫描：探测请求帧、探测响应帧
   + 被动扫描：AP发出信标帧：服务集标识符SSID、AP的MAC地址、支持速率、加密算法和安全配置等参数
   + 重建关联服务：主机更改接入AP
   + 分离服务：终止关联

4. 使用 CSMA/CA 而不是 CSMA/CD 的原因

   CSMA/CA：载波监听、多点接入、碰撞避免，但是并不是避免所有的碰撞而是尽量减少发生碰撞的概率。

   + 无线信道的传输环境复杂，信号强度随着距离的增加衰减非常快，接收方和发送方的信号强度相差非常大，如果要实现碰撞检测对于硬件的要求非常大
   + 存在隐蔽站，使得碰撞检测没有意义<img src="https://wendaocsmaster.github.io/images/blog/image-20230211192344572.png" alt="image-20230211192344572" style="zoom:80%;" />

5. 工作原理

   + DCF帧间间隔：DCF（分布式协调功能），DCF帧间间隔DIFS为$$128us$$，在DCF方式中DIFS用来发送数据帧和管理帧，等待 DIFS 间隔是考虑到可能有其他站的高优先级帧要发送。

     ![image-20230211192405867](https://wendaocsmaster.github.io/images/blog/image-20230211192405867.png)

   + 短帧间间隔SIFS的长度为$$28us$$ ，它是最短帧间间隔用来分隔开属于一次对话的帧。一个站点能够在最短帧间间隔从发送状态转换为发送状态，使用最短帧间间隔的帧有ACK帧和CTS帧

     ![image-20230212091349093](https://wendaocsmaster.github.io/images/blog/image-20230212091349093.png)

   + 虚拟载波监听帧首部中的持续时间字段值指出了源站要占用信道的时间（包括目的站发回确认帧所需的时间），当某个站点检测到正在信道中传送的帧首部中的持续时间字段时，就调整自己的网络分配向量 NAV，NAV指出了完成这次帧的传送且信道转入空闲所需的时间。

     ![image-20230212091922479](https://wendaocsmaster.github.io/images/blog/image-20230212091922479.png)

   + 使用确认机制：由于无线信道的误码率较高，CSMA/CA 协议还需要使用停止等待的确认机制来实现可靠传输

     ![image-20230212092352605](https://wendaocsmaster.github.io/images/blog/image-20230212092352605.png)

   + 退避算法：

     + 使用退避算法的原因：当某个站点在发送帧时候，很可能同时有多个站点在监听信道并等待发送帧。如果不采用退避算法，当上述正在监听并等待发送数据帧的站在监听到信道空闲后几乎同时发送帧而导致信号碰撞。采用退避算法，减少发生碰撞的概率，避免单个站点长时间占用信道。

       ![image-20230212101812038](https://wendaocsmaster.github.io/images/blog/image-20230212101812038.png)

     + 不使用退避算法的情况：检测到信道空闲，且该帧不是成功发送完上一个数据帧之后立即连续发送的数据帧

       使用退避算法的情况：发送数据帧之前信道忙；在每次重传一个帧时；在每次成功发送帧后要连续发送下一个帧时

   + 退避算法的过程：

     1. 在执行退避算法时候，为退避计时器设置一个随机带的退避时间，当退避计时器倒计时为0时就开始发送数据

     2. 当退避计时器没有减小到0时候，信道变为忙，此时冻结退避计时器的值，等信道变为空闲并且等待DIFS时间后，解冻退避计时器

     3. 在进行第$$i$$次退避时，退避时间在时隙$$\{0, 1,...,2^{2+i}-1\}$$ 中随机选择一个，然后乘以基本退避时间（即一个时隙的长度）就可以得到随机的退避时间。当时隙序号达到255时候就不再增加了，对应第六次退避。

        ![image-20230212102146343](https://wendaocsmaster.github.io/images/blog/image-20230212102146343.png)

        ​		以上图为例，站点A、B、C、D、E采用CSMA/CA协议通信，当站点A发送数据时候，站点B、C和E 也要发送数据，此时监测到信道忙，在站点A发送完数据帧信道转为空闲并且经过DIFS后，站点B、C、E分别为各自的退避计时器设置一个随机值进行退避，站点C的退避计时器首先减少到0并且站点C发送数据帧，此时站点B、D监测到信道变为忙，冻结各自的退避计时器。在站点C发送数据帧的时候，站点E也要发送数据帧，但此时监测到信道忙，当站点C发送完数据帧后，站点B、D、E监测到信道变为空闲且经过DIFS后开始各自的动作，站点E设置自己的退避计时器并启动倒计时进行退避，站点B、D解冻各自的退避计时器的值，重新开始倒计时，站点D的退避计时器的值首先减小为0，此时站点D开始发送数据帧，站点B、E监测到信道变为忙，冻结各自的退避计时器的值，当站点D发送完数据帧，站点B、E监测到信道空闲并且经过DIFS后，解冻各自的退避计时器重新开始倒计时，站点E的退避计时器的值首先减少到0，站点E开始发送数据，信道变为忙，站点B冻结自己的退避计时器的值，当站点E发送完数据帧后，站点A检测到信道变为空闲并且经过DIFS后，解冻自己的退避计时器的值，知道退避计时器的值减少为0，开始发送数据帧，在发送完数据帧后，站点B还有连续的帧要发送，在经过DIFS后重新设置一个退避计时器并且开始倒计时，直到B的退避计时器的值减少为0开始发送数据帧。

6. 信道预约：为了进一步降低发生碰撞的概率，802.11无线局域网允许源站对信道进行预约。

   + RTS（Request to send）帧：RTS帧是短的控制帧，它包括源地址、目的地址和本次通信所需的持续时间。

     ![image-20230212104734439](https://wendaocsmaster.github.io/images/blog/image-20230212104734439.png)

   + CTS（Clear to send）：CTS帧时短的响应控制帧，包括本次通信所需的持续时间。

   + 除去目的站和源站的其他各站在收到CTS帧或者数据帧后就推迟访问信道，保证源站和目的站之间的通信不受其他站的干扰

     ![image-20230212105053571](https://wendaocsmaster.github.io/images/blog/image-20230212105053571.png)

   + 使用RTS帧和CTS帧进行信道预约会带来额外的开销，但是因为RTS帧和CTS帧都很短，但是大大减小了碰撞的概率，因为一旦发生碰撞就会导致数据帧重发，浪费时间更多。

   + 信道预约也属于虚拟载波监听机制，只要听到数据帧、RTS帧和CTS帧中的任何一个就能知道信道被占用的时间，而不需要真正监听到信道上的信号。虚拟载波监听机制解决了隐蔽站带来的信号碰撞问题。

     ![image-20230212105744064](https://wendaocsmaster.github.io/images/blog/image-20230212105744064.png)
   
     ![image-20230212105753507](https://wendaocsmaster.github.io/images/blog/image-20230212105753507.png)

## 802.11无线局域网MAC帧

1. 分类：

   + 数据帧：用于各站点之间传输数据
   + 控制帧：ACK帧、RTS帧、CTS帧等
   + 管理帧：信标帧、关联请求帧、身份验证帧等

2. 帧格式：

   ![image-20230212110147924](https://wendaocsmaster.github.io/images/blog/image-20230212110147924.png)

   + 持续期：用于实现CSMA/CA的虚拟载波监听和信道预约机制。在数据帧、RTS帧和CTS帧中使用该字段指明通信所需的持续时间。

   + 序号控制：对帧进行编号

   + 地址1-4取决于帧控制字段汇总的去往DS（到分配系统）和来自DS（来自分配系统）两个字段的值。

     ![image-20230212111245461](https://wendaocsmaster.github.io/images/blog/image-20230212111245461.png)

     地址1：接收地址 

     地址2：发送地址

     地址3：源地址或者目的地址

     地址4：只能用于自组网络

     ![image-20230212111401697](https://wendaocsmaster.github.io/images/blog/image-20230212111401697.png)

     ![image-20230212111822225](https://wendaocsmaster.github.io/images/blog/image-20230212111822225.png)

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
