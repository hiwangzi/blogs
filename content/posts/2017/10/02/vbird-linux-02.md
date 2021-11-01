---
title: 《鸟哥的Linux私房菜》笔记——02. 关于Linux
date: 2017-10-02T12:28:00+08:00
draft: false
summary: ""
tags: ["Linux"]
---

## Unix 历史

* 1969年以前：伟大的梦想——Bell, MIT 与 GE 的「Multics」系统
* 1969年：Ken Thompson 的小型 file server system
* 1973年：Unix 正式诞生（Ritchie等人以 C 语言写出第一个正式 Unix核心）
* 1977年：重要的 Unix 分支——BSD 的诞生
* 1979年：重要的 System V 架构（支持x86）与版权宣告
* 1984年之一：x86 架构的 Minix 操作系统开始编写并于两年后诞生
* 1984年之二：GNU 计划与 FSF(Free Software Foundation) 基金会的成立
* 1988年：图形界面 XFree86(X Window System + Free + x86) 计划
* 1991年：芬兰大学生 Linux Torvalds 的一则简讯

## Linux 的核心版本

```plain
2.6.18-92.el5 
主版本.次版本.释出版本-修改版本 
```

核心被分为两个分支：

* 主、次版本为奇数：发展中版本(development)
  * 如2.5.xx，这种核心版本主要用在测试与发展新功能，所以通常这种版本仅有核心开发工程师会使用。 如果有新增的核心程序代码，会加到这种版本当中，等到众多工程师测试没问题后，才加入下一版的稳定核心中；
* 主、次版本为偶数：稳定版本(stable)
  * 如2.6.xx，等到核心功能发展成熟后会加到这类的版本中，主要用在一般家庭计算机以及企业版本中。 重点在于提供使用者一个相对稳定的Linux作业环境平台。

Linux 是一个操作系统最底层的核心以及其提供的核心工具。他是 GNU GPL 授权模式，所以，任何人均可取得源代码，并且可以修改。

此外，因为 Linux 参考 POSIX 设计规范，于是兼容于 Unix 操作系统，故亦可称之为 Unix Like 的一种。

## 关于开源

Open source 的代表授权为 GNU 的 GPL 授权及 BSD 等等，底下列出知名的 Open Source 授权网页：

* GNU General Public License：http://www.gnu.org/licenses/licenses.html#GPL
  * 目前有version 2, version 3两种版本，Linux使用的是version 2这一版。 

* Berkeley Software Distribution (BSD)：http://en.wikipedia.org/wiki/BSD_license

* Apache License, Version 2.0：http://www.apache.org/licenses/LICENSE-2.0

## 关于闭源

免费的专利软件代表的授权模式有：

* Freeware：
http://en.wikipedia.org/wiki/Freeware
不同于 Free software，Freeware为『免费软件』而非『自由软件！』

* Shareware：
http://en.wikipedia.org/wiki/Shareware
与免费软件有点类似的是，Shareware在使用初期，它也是免费的，但是，到了所谓的『试用期限』之后，你就必须要选择『付费后继续使用』或者『将它移除』。 通常，这些共享件都会自行撰写失效程序，让你在试用期限之后就无法使用该软件。