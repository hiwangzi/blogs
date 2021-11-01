---
title: "什么是 Unix"
date: 2018-02-03T00:00:00+08:00
draft: false
summary: ""
tags: ["Linux"]
---

### 关于内核

* 当计算机启动时，计算机要经历一系列动作，这些动作构成了引导过程。引导过程的最后一个动作是启动一个非常复杂的程序，该程序就被称为**内核（Kernel）**。
* 内核的作用是控制计算机，提供基础服务，是[操作系统](/posts/2017/10/02/vbird-linux-01/#os)的核心。
* 内核有许多种类型，但基本可以分为两大类：
    * 单内核：一个庞大的程序自身可以完成所有的事
    * 微内核：一个非常小的程序只执行最基本的任务，其余通过调用其他程序（称为服务器(server)）实现
* 内核的使用：
    * 大多数 Unix 系统使用的是某种类型的单内核，一些 Unix （例如 OS X，Minix）使用微内核。
    * Linux 是单内核。
    * 补：Linus 阅读了 Andrew Tanenbaum 的《Operating System: Design and Implementation》，该书解释了 Minix 的设计原则。Linus 选择使用单内核设计 Linux，而 Andrew Tanenbaum 设计的 Minix 使用的是微内核。在 Linux 开始引起注意后不久，Tanenbaum 公开批判这种设计决策。直到今天，Tanenbaum 仍然在批评这种单内核设计。
    
### 关于 Unix

* Unix = Unix 内核 + Unix 实用工具
* Unix 过去是属于 AT&T 的商标（必须为大写 UNIX，以下的全大写均指 AT&T 公司的 Unix）
* 现在可以理解为指代任何 “类Unix” 系统
* 关于 “类Unix” 的两种理解：
    * 如果操作系统既包含一个 Unix 内核以及一些 Unix 实用工具，又可以运行能够在其他 Unix 操作系统上运行的程序，那么它就是 Unix 系统
    * 如果理解 Unix 的人说这个系统是 Unix，那么它就是 Unix

### Unix 的历史

* 20世纪70年代的 Unix：由贝尔实验室转向 Berkeley
    * 1974年，Berkeley 的 Bob Fabry 教授获得了一份 UNIX 副本，该校的学生们开始增强该系统。
    * 1977年，Bill Joy 装配了第一版的 Berkeley Unix，被称为 BSD(Berkeley Software Distribution)
    * 1979年，AT&T 公司宣布将 UNIX 作为一个商品销售（UNIX System III("Three"), UNIX System V("Five")）
    * 1979年，所有的 BSD 用户都被要求购买一个 AT&T 公司的许可证，并且之后每年都在提高许可证的价格。因此，BSD 程序员越来越难以忍受 AT&T 公司的束缚。

* 20世纪80年代的 Unix：BSD 与 System V
    * 截止至1980年，美国东海岸 Unix（AT&T 的 UNIX）和西海岸的 Unix（BSD）平分秋色，都发展很快。
    * 此时的 UNIX：AT&T 目标是使 UNIX 成为一个成功的商业产品，只面向能够为许可证付大量金钱的公司。1982年，发行了 UNIX System III。1983年，发行了 System V，到年末，System V 安装了 45,000 份。1984年，System V Release 2(SVR2) 发行时，大约安装了 100,000 份。
    * 此时的 BSD：1980年，Berkeley 的 Bob Fabry 教授接到美国国防部高级研究计划局（DARPA，补：1972年前，该机构名为 ARPA，Internet的祖先 Arpanet 也由该机构创造）的一个大合同。Fabry 随后建立了 CSRG(Computer System Research Group) ，该小组一直延续到 1994 年，且在这段时间，对 BSD 和 Unix 在全球的发展产生了重要影响。随后该小组发布的 Unix 版本都受到了学术界与研究社区的高度关注。1982年，4.1BSD支持TCP/IP，成为 Internet 的基础。1983年，4.2BSD发布，全球已有1000份安装。
    * 到 1985 年，Unix 流派就是以上两类。其他形式的 Unix 也都派生自以上两类。
    * 在20世纪80年代末，Unix世界的两大特征：Unix 总体的快速增长和不同类型 Unix 的增殖扩散。

* AT&T 公司的限制
    * 1979年，AT&T不允许其公司之外的人查看 UNIX 的源代码
    * 1984年之一：x86 架构的 Minix 操作系统开始编写并于两年后诞生
    * 1984年之二：GNU 计划与 FSF(Free Software Foundation) 基金会的成立
    * 1991年：芬兰大学生 Linux Torvalds 发布了一则简讯，Linux 世界自此开始，随后发展出了数百个发行版
    * 1992年：Bill Jolitz 替换了最后的 6 个 AT&T 公司的内核文件，发行了一个完全与 AT&T 无关的 Unix：386/BSD。后来更名为 FreeBSD。后来衍生出了 NetBSD(可移植到许多种类的计算机)，OpenBSD(关注安全与密码学)。

### 为何 Linux 成为了更加流行的 Unix？

* Linux 是基于 GNU GPL 许可发行的，而 GPL 禁止任何人使用 Linux 创建及发行专有系统。而 BSD 许可证远没有 GPL 严格，在 BSD 许可证之下，允许使用部分 BSD 创建新产品而不共享新产品，因此这种情况下，其他人无法从新产品获得好处，也无法使用与修改新产品（但也因为 BSD 许可证的灵活，所以应用也非常广泛）。
* Linux 比 FreeBSD 更成功的关键在于发行时机，Linux Torvalds 在 1991 年发行了 Linux 内核，而完全开放源代码的 386/BSD 直到 1992 年才发行。
