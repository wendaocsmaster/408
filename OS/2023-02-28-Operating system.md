---
layout: post
title: 操作系统
categories: [操作系统]
description: OS
keywords: Operating system 
---

# 概述

1. 何为操作系统？

   目前对于操作系统并没有公认的非常精确的定义。大部分资料给出的定义如下：首先，操作系统是一种系统软件，能够管理硬件资源、控制程序运行、改善人机界面和为应用软件提供支持的一种系统软件。从这个意义上讲，目前的微信也应属于操作系统，例如，微信能够支持各种小程序，为这些小程序提供硬件资源的分配，能够控制各小程序的运行，同时各种UI设计改善了人接交互界面并且为这些小程序提供支持；Office运行在windows操作系统执行环境中，Web应用运行在处理器的Chrome OS系统执行环境中，某些嵌入式环境中，操作系统与应用程序紧密结合在一起<font color = red>（操作系统架构为简单结构）</font>。因此在不同的场景中操作系统的边界是不同的。

   如果以软件执行时候CPU的状态进行区分，真正的操作系统是操作系统kernel，这是运行在CPU内核态的，而其他软件如微信等是运行在用户态的，从这个意义上讲微信不属于操作系统。通常来讲操作系统需要包括

   + 操作系统内核：向下管理硬件资源，向上对各应用程序提供服务
   + 系统工具和软件库：为操作系统提供基本功能的软件
   + 用户接口：GUI和CLI

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301082600368.png" alt="image-20230301082600368" style="zoom:80%;" />

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301080341308.png" alt="image-20230301080341308" style="zoom:67%;" />

2. 不同阶段的操作系统要干什么？

   操作系统最直接的需求就是要管理硬件、支持软件应用程序。在单用户系统时代，使用纸带等进行程序的输入和输出，此时的操作系统就是装载器和程序库的集合，但是昂贵的CPU硬件设备大量时间处于等待过程中，利用率太低，为了解决CPU利用率太低的问题，提出了批处理系统，使用磁盘、磁带等存储设备进行程序的输入和输出，大大减少了CPU的等待时间，提高了CPU利用率，此时的操作系统就是装载器、程序控制器和输出处理器的集合。

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301080613424.png" alt="image-20230301080613424" style="zoom:18%;" />

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301080429833.png" alt="image-20230301080429833" style="zoom:25%;" />

   

   但是此时的操作系统对于各应用程序的支持仍然是各程序按照加载到内存中的顺序来执行，内存中同一时刻仅允许有一个程序，为了进一步提高系统的任务吞吐量和CPU利用率有了多道批处理系统，同一时刻在内存中有多个程序，使得各应用程序能够交替使用CPU，此时的操作系统就是装载器、程序调度、内存管理和输出管理的集合。

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301081020636.png" alt="image-20230301081020636" style="zoom: 30%;" />

   为了满足用户和主机的交互以及多任务、多用户的支持提出了分时操作系统，主机以时间片为单位轮流分配给各用户或者终端使用，满足了用户和主机的交互、用户对主机的独占感知以及多用户共同使用同一主机时候的协调，UNIX。此时的操作系统就是装载器、程序调度、内存管理、中断处理等的集合。

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301082159602.png" alt="image-20230301082159602" style="zoom:30%;" />

3. 应用程序、操作系统、硬件设备

   随着硬件设备性能的提高，为了提高硬件设备的利用率要求操作系统能够在宏观上支持多个应用程序同时执行<font color = red>（并发）</font>，但是对于单CPU主机，各个应用程序又不可能在同一时刻使用CPU等设备<font color=red>（互斥共享）</font>，同时在微观上各应用程序是走走停停，完成的时间不能确定<font color = red>（异步）</font>，但是各应用程序感知到的是该程序“独占”用户主机，某些应用程序对于内存的要求可能远大于实际的物理内存，但是操作系统依然能够支持该程序的运行<font color = red>（虚拟）</font>。

   虚拟化、并发、持久性——[Operating Systems: Three Easy Pieces (wisc.edu)](https://pages.cs.wisc.edu/~remzi/OSTEP/)

4. 用户需要操作系统提供怎样的服务？

   + 高效和易用？

     在coding中会有这样的例子，一行代码实现人脸识别等，但是这样的易用性是建立在对底层实现高度封装的基础之上的，通常封装层数越高，对于使用者来说越方便，但是却牺牲了高效，最上层的函数需要逐层向下最终通过硬件实现。操作系统的实现也是一样的，往往需要在高效和易用之间寻找平衡。

   + 强大的操作系统服务和简单的接口？

     如上述案列，一行代码可以实现人脸识别，但是牺牲了性能，同时由于函数高度封装，使用会受到很大的限制，能够实现图像的人脸识别却可能无法实现视频的人脸识别。如果接口过于简单操作系统能够提供的服务会受到极大的限制，而无法为上层提供丰富的服务，如果要提供强大的服务，就必须实现各种各样的接口。操作系统往往需要在强大的服务和简单接口之间寻找平衡。

   + 灵活性和安全性

     通常强调安全时候需要各种各样的检查和限制，这就限制了其灵活性。同样操作系统往往也需要在灵活性和安全性之间寻找平衡。

5. 操作系统架构

   + 简单结构：应用和OS紧耦合在一起，没有拆分模块，早起的MS-DOS以及一些简单的嵌入式设备中就是这种结构。这种结构设计没有安全性保护，在用户态运行的应用程序直接和内核态运行的Kernel紧耦合，对于真个系统和设备时非常危险的。

   + 单体分层结构：将单体操作系统（Monolithic OS）划分为多层，每层建立在低层之上，使用下层提供的服务，但是随着用户对操作系统的需要越来越多，对操作系统中不断加入新的功能使得这些层次已经不像设计之初一样层次分明。

     <img src="https://wendaocsmaster.github.io/images/blog/image-20230301125659211.png" alt="image-20230301125659211" style="zoom: 33%;" />

   + 微内核结构：尽可能把内核的功能移到用户空间，例如文件系统、内存管理、IO子系统、网络协议栈和设备驱动等，内核仅提供硬件抽象层HAL(Hardware Abstraction Layer)和本地过程调用LPC(Local Procedure Call)，以进程之间的通信为例，内核对于进程之间的通信和访问时不信任的，进程之间的同步和通信必须通过内核来进行，这样提高了系统整体的安全性和灵活性，但是这样使得通信过程每次都经过内核，降低了效率，对于系统产生了不可忽视的性能的影响。当安全和性能之间产生矛盾的时候，大多数用户往往会选择性能。对于核电站和医疗设备中需要高安全可靠的系统时候采用这种架构。

     <img src="https://wendaocsmaster.github.io/images/blog/image-20230301131014393.png" alt="image-20230301131014393" style="zoom:40%;" />

   + 外核结构(Exokernel)：外核提供对不同物理设备的虚拟化，并且分配物理资源给多个应用程序，并让每个程序决定如何处理这些资源。将应用程序和其所需要的的协议栈等进行安全绑定，这样对于不同的应用程序能够实现定制的内核，这样协议栈等和应用程序紧耦合在用户态下运行，从安全上讲，即使有恶意程序也只能破坏属于自己的那部分资源等，从性能上讲，由于协议栈等和应用程序紧耦合，应用程序可以直接通过函数调用的方式使用，比通过系统调用使用更加高效。例如对于网络通信应用需要通信协议栈等，而对于科学计算的应用程序可能并不需要网络通信协议栈而需要更大的内存空间。目前并没有在产业界中实现。

     <img src="https://wendaocsmaster.github.io/images/blog/image-20230301132945119.png" alt="image-20230301132945119" style="zoom:40%;" />

   + 虚拟机机构

     对于上述外核结构的idea出现在了云服务中，对硬件进行虚拟化同时对上层的应用提供支持。是对外核架构的扩展和延伸，不过不是对单个应用提供支持，而是直接对VM进行支持，应用程序运行在VM中，对硬件资源进行充分利用。

6. 为了便于管理和实现操作系统对于各硬件资源进行了不同的抽象

   + CPU：进程
   + Storage：文件
   + 内存：地址空间

## 参考资料

[操作系统课在线幻灯片链接](https://www.yuque.com/xyong-9fuoz/qczol5/glemuu?)

[os-lectures2023: OS Lectures 2022 Spring ](https://learningos.github.io/os-lectures/)

[rust-based-os-comp2023: 2023开源操作系统训练营](https://learningos.github.io/rust-based-os-comp2022/)

[OS video](https://github.com/wendaocsmaster/rust-based-os-comp2023/blob/main/relatedinfo.md)

[lab-uCore-Tutorial-Guide-2023S 文档 (learningos.github.io)](https://learningos.github.io/uCore-Tutorial-Guide-2023S/)

[lab-rCore-Tutorial-Book-v3 3.6.0-alpha.1 文档 (rcore-os.cn)](http://rcore-os.cn/rCore-Tutorial-Book-v3/index.html)

[Introduction · GitBook (learningos.github.io)2020](https://learningos.github.io/ucore_os_webdocs/)

[ucore-os-docs](https://github.com/csmasterpath/ucore_os_docs)

[Operating Systems: Three Easy Pieces (wisc.edu)](https://pages.cs.wisc.edu/~remzi/OSTEP/)

[Index of /~remzi/OSTEP/Chinese (wisc.edu)](https://pages.cs.wisc.edu/~remzi/OSTEP/Chinese/)

[Introduction  uCore Lab Documents (gitbooks.io)2015](https://objectkuan.gitbooks.io/ucore-docs/content/)

[操作系统-rCore-Tutorial-Book-v3 3.6.0-alpha.1 文档 (rcore-os.cn)](http://rcore-os.cn/rCore-Tutorial-Book-v3/chapter0/index.html)
