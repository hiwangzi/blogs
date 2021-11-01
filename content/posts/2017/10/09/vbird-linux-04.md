---
title: 《鸟哥的Linux私房菜》笔记——04. 简单命令行
date: 2017-10-09T18:25:00+08:00
draft: false
summary: ""
tags: ["Linux"]
---

## 键入命令

```bash
[zill@hiwangzi.com ~]$ command  [-options]  parameter1  parameter2 ...
                        指令        选项        参数(1)     参数(2)
```

注意：有时也可以使用 ```+``` 放置于选项或参数之前

例如：

```bash
zill@zill-pc:~$ date +%Y/%m/%d
2017/10/09
zill@zill-pc:~$ date +%H:%M
17:32
```

## 几个程序
* 显示日期与时间的指令： date
* 显示日历的指令： cal
* 简单好用的计算器： bc（进入交互界面后 scale=number 设置小数点位数）

## 几个按键
* [Tab]：主要用于自动补全
* [Ctrl]-c：中断当前程序
* [Ctrl]-d：通常代表「键盘输入结束（End Of File, EOF 或 End Of Input）」的意思（可以代替手动输入 `exit`）
* [Shift]+{[PageUP]|[Page Down]}：向前翻页|向后翻页

## 关于帮助文档

### 1. man

表格的第一行，可以看到「DATE(1)」，其中(1)代表「一般使用者可以使用的指令」。

常见代号及含义（可以通过 man man 获得更详细的说明）：

|代号|代表内容|
|---|-------|
|1|使用者在shell环境中可以操作的指令|
|2|系统核心可调用的函数与工具等|
|3|一些常用的函數(function)与函数库(library)，大部分为C的函数库(libc)|
|4|硬件的说明，通常在/dev下的文件|
|5|设置文件或者是某些文件的格式|
|6|游戏(games)|
|7|惯例与协定，例如Linux文件系统、网络协议、ASCII code等等的说明|
|8|系统管理员可以使用的管理指令|
|9|跟kernel有关的文件|

可以使用 ```man -f 指令``` 查找相关指令（名称完全相同）。
可以使用 ```man -k 指令``` 搜索相关指令。

### 2. info page

Linux 里额外提供的一种线上求助的方法，和 man 的用途差不多，但与man page输出全部信息不同，info page阅读起来更友好。

### 3. /usr/share/doc

## 系统状态

* 查看在线的用户：`who`
* 查看网络的连接状态：`netstat -a`
* 查看后台程序：`ps -aux`

## 关机

* 将数据写入硬盘的指令： `sync`（目前的 `shutdown`/`reboot`/`halt` 等等指令均已在关机前对 `sync` 进行了调用）
* 常用关机指令： `shutdown`
* 重新开机、关机： `reboot`, `halt`, `poweroff`
* 还可以使用管理工具 systemctl 关机
    ```
    [root@study ~]# systemctl [指令]
    指令項目包括如下：
    halt       進入系統停止的模式，螢幕可能會保留一些訊息，這與你的電源管理模式有關
    poweroff   進入系統關機模式，直接關機沒有提供電力喔！
    reboot     直接重新開機
    suspend    進入休眠模式
    ```
