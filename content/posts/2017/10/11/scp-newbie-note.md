---
title: scp 命令简明介绍
date: 2017-10-11T10:53:00+08:00
draft: false
summary: ""
tags: ["Linux"]
---

> 安全复制（英语：Secure copy，缩写SCP）是指在本地主机与远程主机或者两台远程主机之间基于Secure Shell（SSH）协议安全地传输电脑文件。“SCP”通常指安全复制协议或者程序本身。[安全复制 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E5%AE%89%E5%85%A8%E5%A4%8D%E5%88%B6)

* 其使用方法类似于 ```cp``` 命令。

* 复制文件或目录命令：
    * 复制文件：
        1. 将本地文件拷贝到远程 ```scp 文件名 --用户名@计算机IP或者计算机名称:远程路径```
        2. 从远程将文件拷回本地 ```scp --用户名@计算机IP或者计算机名称:文件名 本地路径```

    * 复制目录：
        1. 将本地目录拷贝到远程 ```scp -r 目录名 用户名@计算机IP或者计算机名称:远程路径```
        2. 从远程将目录拷回本地 ```scp -r 用户名@计算机IP或者计算机名称:目录名 本地路径```

* 注意，如果远程主机使用非默认端口22，可以在命令中指定。例如，从远程主机复制一个文件到本地。
    ```shell
    scp -P 2222 user@host:directory/SourceFile TargetFile
    ```