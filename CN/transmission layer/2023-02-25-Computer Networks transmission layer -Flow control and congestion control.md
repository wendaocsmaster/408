---
layout: post
title: 计算机网络传输层流量控制和拥塞控制
categories: [计算机网络]
description: 传输层
keywords: computer networks network layer 
---

## 流量控制和拥塞控制

因为接收方的处理能力和发送方的处理能力不同，当发送方发送的数据量过多时候，接收方以自己的接收能力控制发送方的发送速率。具体到TCP协议中就是接收方告诉发送方自己的接收窗口大小，发送方设置自己的发送窗口，发送窗口的大小不大于接收方的接收窗口大小，只有落在发送窗口内的数据才能被发送出去。

拥塞控制是发送方根据自己对网路状态的感知，通过拥塞控制算法，控制自己的发送速率。

实际中，发送方的发送窗口是拥塞窗口和接收方接收窗口中的最小值。

![image-20230225103214468](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.png)

### 流量控制

1. 在建立连接时候设置窗口大小

![image-20230225104055744](https://wendaocsmaster.github.io/images/blog/image-20230225104055744.png)

2. 通信

   ![image-20230225104322781](https://wendaocsmaster.github.io/images/blog/image-20230225104322781.png)

   ![image-20230225104436124](https://wendaocsmaster.github.io/images/blog/image-20230225104436124.png)

3. 非零窗口通知报文和主动探测报文

   ![image-20230225104606420](https://wendaocsmaster.github.io/images/blog/image-20230225104606420.png)

   ![image-20230225104756350](https://wendaocsmaster.github.io/images/blog/image-20230225104756350.png)



### 拥塞控制

TCP中的拥塞控制算法主要分为四个部分，慢开始，拥塞避免，快重传、快恢复

![image-20230225105211095](https://wendaocsmaster.github.io/images/blog/image-20230225105211095.png)

+ 当$$cwnd < ssthresh$$时，使用慢开始算法。
+ 当$$cwnd > ssthresh$$时，停止使用慢开始算法而改用拥塞避免算法。
+ 当$$cwnd = ssthresh$$时，均可。

判断网络出现拥塞的依据：没有按时收到应当到达的TCP确认报文段而产生了超时重传

#### 慢开始

![image-20230225105646278](https://wendaocsmaster.github.io/images/blog/image-20230225105646278.png)

### 拥塞避免

![image-20230225105802448](https://wendaocsmaster.github.io/images/blog/image-20230225105802448.png)

仅使用慢开始和拥塞避免示意图如下

![image-20230225105921895](https://wendaocsmaster.github.io/images/blog/image-20230225105921895.png)

### 快重传

为了使得发送方尽早知道个别TCP报文段的丢失，并尽早进行重传，因此要求接收方不再使用捎带确认的累计确认，而是对于发送方发送的数据立即发出确认，即使收到失序的报文段，也要对自己已经收到的按序接收的报文段发送确认，发送方一旦连续接收到数个重复的确认，就将相应的报文段重传，而不是等超时重传，实际实现中一般选择接收到三个重复确认后重传。

![image-20230225110534110](https://wendaocsmaster.github.io/images/blog/image-20230225110534110.png)



### 快恢复

实际中，数据报可能在网络传输过程中丢失了，但是此时网络并没有发生拥塞，而此时执行慢开始算法显然是不合适的，因此采用快恢复算法。将发送方的发送窗口和拥塞门限值设置为方发生重传时刻的一般，并开始执行拥塞避免算法。

![image-20230225111114271](https://wendaocsmaster.github.io/images/blog/image-20230225111114271.png)

## 参考资料

[网络是怎样连接的](https://book.douban.com/subject/26941639/)(日)*户根勤著*; *周自恒译*. 责任者, 户根勤; 周自恒. 出版发行, 北京: 人民邮电出版社,2017

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/blog/093421-01.jpg)
