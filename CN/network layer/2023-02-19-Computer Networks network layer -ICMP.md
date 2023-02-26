---
layout: post
title: 计算机网络网络层网际控制报文协议ICMP
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## 网际控制报文协议ICMP

1. 为了解决在IP数据报传输过程中出现差错后向源主机报告等问题，从而更有效地转发IP数据报以及提高IP数据报交付的成功率，在网际层实现了ICMP网际控制报文协议。主机或者路由器可以使用ICMP来发送差错报告报文或者询问报文。ICMP报文被封装在IP数据报中发送。

2. ICMP报文的分类

   + 常见的差错报告报文

     终点不可达：当路由器或者主机不能交付IP数据报时候，向源点发送终点不可达报文，具体可在根据ICMP的代码字段分为网路不可达、目的主机不可达、目的协议不可达、目的端口不可达等13种

     源点抑制：当路由器或者主机由于拥塞而丢弃IP数据报时，就向发送该IP数据报的源点发送源点抑制报文，使得源点将IP数据报的发送速率降低。

     超时：超过TTL的设置值

     参数问题：检测到IP数据报首部误码等

     改变路由（重定向）：有更优的路由选择。

   + ICMP询问报文

     回送请求和回答：测试目的站是否可达及了解有关状态

     时间戳请求和回答：同步时钟和测量时间

3. 不发送ICMP差错报告的情况

   + 对于ICMP差错报告报文不再发送ICMP差错报告报文
   + 对于第一个分片的IP数据报片的所有后续数据报片都不发送ICMP差错报告报文，只有第一个分片拥有该IP数据报的传输层信息，如端口等
   + 对于多播地址的IP数据报都不发送ICMP差错报告报文
   + 对于特殊地址如127.0.0.0或者0.0.0.0的IP数据报都不发送差错报告报文

4. 应用：

   win: ping pathping(tracert) 应用层直接使用ICMP协议

   unix:traceroute 运输层使用UDP，网际层使用ICMP

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
