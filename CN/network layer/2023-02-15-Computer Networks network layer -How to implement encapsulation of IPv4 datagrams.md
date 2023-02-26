---
layout: post
title: 计算机网络网络层IPv4数据报封装
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## IPv4数据报封装格式

1. IPv4数据报首部格式

   ![image-20230215002757245](https://wendaocsmaster.github.io/images/blog/image-20230215002757245.png)

2. 版本：长度为4bit，用来表示IP协议的版本，通信双方使用的IP协议版本必须一致。IPv4

3. 首部长度：长度为4bit，以4B为单位，用来表示IPv4数据报的首部长度。取值最小为0101，即十进制5，再乘以4就是IPv4数据报首部最小长度20B，最大取值为1111，即十进制的15，再乘以4就是IPv4数据报的首部最大长度为60B。

4. 可选字段：长度1-40字节不等，用于排错及安全措施等功能，很少被使用。

5. 填充：确保IPv4数据报的首部长度为4B的整数倍，使用全0进行填充。

   ![image-20230215003831114](https://wendaocsmaster.github.io/images/blog/image-20230215003831114.png)

6. 区分服务：利用该字段的取值可提供不同等级的服务质量，一般不使用

7. 总长度：长度为16bit，用来表示IPv4数据报的长度，即首部长度加数据载荷长度，最大取值为65535

   ![image-20230215003900780](https://wendaocsmaster.github.io/images/blog/image-20230215003900780.png)

8. 片偏移：IP数据报过长时候，由于规定以太网MAC帧的数据载荷最大为1500B，所以必须对IPv4数据报进行分片。片偏移的长度为13bit，该字段的单位为8B，表示分片IPv4数据报的数据载荷在原IPv4数据报的位置。<font color =red>片偏移的值必须为整数</font>

9. 标识：属于同一IPv4数据报的各分片具有相同的标识。

10. 标志：最低位MF（More Fragment）1，标识本分片后面还有分片；0表示本分片后面没有分片。

    中间位DF（Don't Fragment） 1，表示不允许分片； 0表示允许分片

    最高位为保留为必须为0

    ![image-20230215004722053](https://wendaocsmaster.github.io/images/blog/image-20230215004722053.png)

    ![image-20230215004845249](https://wendaocsmaster.github.io/images/blog/image-20230215004845249.png)

11. 生存时间TTL：每经过一个路由器其值减少1，为0的时候生命周期结束。防止数据报在有环路的的网络中反复兜圈浪费网络资源。

12. 协议：指明应将IP数据报向上交给哪个协议处理。6是TCP，17是UDP

13. 首部检验和：二进制反码算术运算求和
$$
0+0=0\\
0+1=1\\
1+1=0,\quad and\quad generate\quad a\quad rounding \quad 1 \quad to \quad add\quad to \quad the \quad next\quad column\\
If \quad the\quad  highest\quad  bits\quad  are\quad  added\quad  together\quad  and\quad  then\quad  rounded,\quad  the\quad  final\quad  result\quad  is\quad  added\quad  by\quad  1
$$

![image-20230215005600238](https://wendaocsmaster.github.io/images/blog/image-20230215005600238.png)


<img src="https://wendaocsmaster.github.io/images/blog/image-20230215010902538.png" alt="image-20230215010902538" style="zoom:67%;" />

![image-20230215012012174](https://wendaocsmaster.github.io/images/blog/image-20230215012012174.png)


​    

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
