---
layout: post
title: 计算机网络物理层
categories: [Blog, 计算机网络]
description: 物理层解决的是网络接口和如何表示比特信号0和1的问题
keywords: computer networks physical layer
---

## 概览

1. 物理层要解决的问题？

   + 网络接口如何规定？确保各厂家生产的网络接口能够通用，目前最多的是 RJ45 接口，也就是所说的水晶头接口
   + 如何表示比特信号0和1？编码问题，传统以太网使用的是曼彻斯特编码

2. 物理层的接口特性：

   + 机械特性：形状、尺寸、引脚数目和排列等
   + 电气特性：电压的信号范围、传输速率、距离限制、阻抗匹配等
   + 功能特性：规定接口电缆的各条吸纳号线的作用
   + 过程特性：各信号的<font color = red>时序关系</font>

3. 物理层下的传输媒体：

   导向型：同轴电缆、双绞线（无屏蔽双绞线 UTP、屏蔽双绞线 STP）、光纤（多模光纤 [光电二极管] 、单模光纤 [半导体激光器和激光检波器]）

   非导向性：微波、无线电波 [无线 WIFI 2.4G 和 5.8G]、红外线、大气激光、可见光 [LIFI]

4. 传输方式：

   + 串行传输：只有一条数据线一个比特接一个比特发送，造价低适用于远距离传输

   + 并行传输：多个比特在多条数据线上同时传输，造价高适用于短距离传输，一般用于计算机内部各部件之间，例如CPU和存储器之间的传输

   + 通常网卡将数据发送到链路上，需要将CPU和存储器发来的数据进行并串转换

     ![image-20230204102628867](https://wendaocsmaster.github.io/images/blog/image-20230204102628867.png)

5. 同步传输和异步传输：

   + 同步传输是指接收方在比特信号的中间时刻进行采样

     外同步：增加一条时钟信号线，但是由于不可能生产出两条完全一样的信号线，同时两条信号线的实际通信情况也可能有所不同，如果通信双方的距离比较长，会导致接收双方的时钟频率的误差累计造成比特信号采样时刻的严重偏移。

     内同步：在发送端将时钟信号编码到数据信号中一起发送。例如：曼彻斯特编码和差分曼彻斯特编码、归零编码

   + 异步传输：指的是字节之间异步，即字节之间的时间间隔不固定，但是在字节中的每个比特任然要同步，即各个比特的持续时间是相同的。这种传输方式需要在每个字节的开始和结尾增加定界符。

     

6. 单工通信：无线广播、电视，固定发送方和接收方

   半双工通信：通信线路中同一时刻只允许有一个发送方：对讲机

   全双工通信：手机



## 编码与调制

1. 编码与调制：

   消息：文字、图片、音频、视频

   数据：运送消息的实体

   信号：数据的电磁表现

   数字基带信号：计算机发出的数字信号，数字基带信号经过基带调制（编码）进入数字信道，经过带通调制进入模拟信道

   消息:arrow_right:数据:arrow_right:信号:arrow_right:数字基带信号

2. 码元：表示不同离散值得<font color = red>基本波形</font>

3. 常用编码方式：

   + 不归零制：双极性不归零编码

     ![image-20230204105926460](https://wendaocsmaster.github.io/images/blog/image-20230204105926460.png)

   + 归零制：双极性归零编码

     ![image-20230204110021984](https://wendaocsmaster.github.io/images/blog/image-20230204110021984.png)

   + 曼彻斯特编码：

     ![image-20230204110055375](https://wendaocsmaster.github.io/images/blog/image-20230204110055375.png)

   + 差分曼彻斯特编码：

     ![image-20230204110128054](https://wendaocsmaster.github.io/images/blog/image-20230204110128054.png)

     **差分曼彻斯特编码具有更优秀的抗干扰能力，**<font color = red>显然在连续大连传输0或1的情况下，差分曼彻斯特变吗信号比曼彻斯特编码信号的变化要少的多，在复杂通信环境下，检测有无跳变比检测跳变的方向更不容易出错，因此这种编码方式更容易检测，在传输介质接线错误导致高低电平信号翻转的情况下仍然有效。</font>

4. 基本带通调制方法和混合调制方法：

   基本的带通调制方法：调幅、调频、调相

   混合调制方法：频率和相位相关（二选一）、振幅

   通常选用正交振幅调制 QAM （相位和振幅）

   码元与比特信号的对应关系不是随意的，应使用格雷码编码，格雷码在传输相邻的比特信号时候只需要变一个bit位，大大减少了出错概率，同时提高了传输效率。



## 信道的极限容量

1. **<font color = red>码间串扰：</font>**数字信号的高频分量在传输时候收到衰减甚至不能通过信道，则接收端收到的波形前沿和后沿就变得不那么陡峭，每一个码元所占的时间界限也不再明确。这样接**收端收到的信号波形就失去了码元之间的清晰界限**，这种现象称为码间串扰。

2. **<font color = red>奈氏准则</font>**：理想的低通信道的最好码元传输速率为 $$C = 2W Baud$$，$$W$$为平道带宽单位 $$Hz$$， $$Baud$$：波特，码元/秒

   码元的传输速率又称为波特率、调制速率、或者符号速率

   当一个码元只携带 1bit 信息时候，波特率和比特率在数值上是相等的，当一个码元携带 n bit的信息时候，波特率转换成比特率时候要乘以 n

3. **<font color = red>香农公式：</font>**$$C = Wlog_2(1+ \frac{S}{N})$$

   $$C$$： 信道的极限数据传输速率  $$bit/s$$

   $$W$$：信道的频率带宽  $$Hz$$

   $$S$$ ： 信道内所传信号的功率

   $$N$$： 信道内的高斯噪声功率

   $$\frac{S}{N}$$： 信噪比，使用分贝（$$dB$$）为单位，信噪比（$$dB$$）= $$10log_{10}\frac{S}{N}$$

4. 根据奈氏准则和香农公式，在信道频率宽带一定的条件下，要想提高信息的传输速率，就必须**<font color = red>采用多元制调制技术，并努力提高信道中的信噪比</font>**

## 信道的复用技术

1. 频分复用：FDM

   ![image-20230204114039158](https://wendaocsmaster.github.io/images/blog/image-20230204114039158.png)

2. 时分复用：TDM

   ![image-20230204114107437](https://wendaocsmaster.github.io/images/blog/image-20230204114107437.png)

3. 波分复用：WDM（光的频分复用）

   ![image-20230204114215378](https://wendaocsmaster.github.io/images/blog/image-20230204114215378.png)

4. 码分复用：CDM，CDMA 码分多址

   案例一：某个站点要发送数据

   + ![image-20230204114510258](https://wendaocsmaster.github.io/images/blog/image-20230204114510258.png)
   + ![image-20230204114650636](https://wendaocsmaster.github.io/images/blog/image-20230204114650636.png)
   + ![image-20230204114718795](https://wendaocsmaster.github.io/images/blog/image-20230204114718795.png)
   + ![image-20230204114750172](https://wendaocsmaster.github.io/images/blog/image-20230204114750172.png)

案例二：知道某个站点的码片序列并发送数据给该站点

![image-20230204115040568](https://wendaocsmaster.github.io/images/blog/image-20230204115040568.png)

![image-20230204115243089](https://wendaocsmaster.github.io/images/blog/image-20230204115243089.png)

![image-20230204115135955](https://wendaocsmaster.github.io/images/blog/image-20230204115135955.png)

![image-20230204115256102](https://wendaocsmaster.github.io/images/blog/image-20230204115256102.png)

![image-20230204115307827](https://wendaocsmaster.github.io/images/blog/image-20230204115307827.png)

![image-20230204115318816](https://wendaocsmaster.github.io/images/blog/image-20230204115318816.png)







## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
