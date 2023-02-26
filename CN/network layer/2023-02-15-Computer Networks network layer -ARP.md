---
layout: post
title: 计算机网络网络层地址解析协议ARP
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## 地址解析协议ARP

1. 在数据报的传送过程中，数据报的源IP地址和目的IP地址保持不变，数据帧的源MAC地址和目的MAC地址逐链路或者逐网络改变

   ![image-20230215181705566](https://wendaocsmaster.github.io/images/blog/image-20230215181705566.png)

2. 网际层使用IP地址进行寻址而不是使用MAC地址寻址，使得网络中各路由器的路由表记录的数量大大减少。路由器接收到IP数据报后根据其中的目的地址的网络号部分决定如何转发，但是在查表转发时，只能查到下一跳的路由器地址或者主机地址（特定路由），无法查明该IP地址对应的MAC地址，在数据链路层对数据报封装成帧时缺少目的MAC地址，由此引出了地址解析协议ARP。

   ![image-20230215182349995](https://wendaocsmaster.github.io/images/blog/image-20230215182349995.png)

3. ARP地址解析协议：获取本网络内特定IP地址的MAC地址

   ARP告诉缓存表：维护本网络内各IP地址和MAC地址的映射，但是主机MAC地址并不是固定不变的，主机可能更换网卡，可能发生主机移动等。动态类型是由各主机维护，每隔一定时间就失效需要通过广播的方式更新，静态类型是通过人为设置的MAC地址与IP地址的绑定。

   ![image-20230215183115840](https://wendaocsmaster.github.io/images/blog/image-20230215183115840.png)

4. 如果对于特定的IP地址在ARP告诉缓存表中并未找到IP地址到MAC地址的映射，主机或者路由器会发送广播帧询问该IP地址的MAC地址，本网络内收到该广播帧的主机或者路由器向上层提交，当发现ARP报文中的目的IP地址与自己的IP地址一致时，发回ARP响应数据报。

5. ARP存在网络安全问题，如ARP欺骗。

   ![image-20230215190716028](https://wendaocsmaster.github.io/images/blog/image-20230215190716028.png)

   ​		如上图，网络192.168.0.0网络中共有三台主机，分别为A、B、C，路由器192.168.0.1作为该网络的默认网关。主机A想要访问网络192.168.1.0/24中的资源，但是此时主机A的ARP高速缓存表中并没有路由器的MAC地址，无法将数据报封装成帧发送给路由器，此时主机A广播发送ARP请求报文，询问192.168.0.1对应的MAC地址，本网络内的所有主机都能收到此ARP请求报文。收到ARP请求报文的主机C发现询问的IP地址不是自己的IP地址，丢弃该数据报；收到ARP请求报文的路由器立即发回ARP响应报文，此时主机A正确接收到了默认网关的MAC地址，30s后收到ARP请求报文的主机B发回ARP欺骗响应报文，此时主机A会更新ARP高速缓存表中的内容，将IP地址192.168.0.1对应的MAC地址修改为B的MAC地址，此时就会造成A发出的所有数据帧都会经过主机然后转发给路由器，B就可以窃取A的通信内容。

   ![image-20230215192835979](https://wendaocsmaster.github.io/images/blog/image-20230215192835979.png)

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
