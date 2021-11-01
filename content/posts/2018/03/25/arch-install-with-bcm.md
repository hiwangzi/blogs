---
title: "安装 Arch 中遇到的 Broadcom b43(BCM43228) 网卡问题"
date: 2018-03-25T00:00:00+08:00
draft: false
summary: "之前按照《以官方 Wiki 的方式安装 ArchLinux》，并结合官方文档，成功的在 VirtualBox 环境下安装好了 Arch。虚拟机限于内存、显卡等因素，体验不够好，因此今天准备在旧笔记本实机安装。在第一步联网过程中，就遇到了阻碍，特此记录。"
tags: ["Linux"]
---

之前按照以 [官方 Wiki 的方式安装 ArchLinux](https://www.viseator.com/2017/05/17/arch_install/)，并结合 [官方文档](https://wiki.archlinux.org/index.php/installation_guide)，成功的在 VirtualBox 环境下安装好了 Arch。虚拟机限于内存、显卡等因素，体验不够好，因此今天准备在旧笔记本实机安装。在第一步联网过程中，就遇到了阻碍，特此记录。

## 查看网卡类型

在之前的虚拟机环境下，采用 NAT 模式联网，网卡自动识别并加载，只需要执行 `dhcpcd` 获取 IP 后即可成功连接互联网。但这次，系统并不能自动驱动硬件工作，所以我们需要先检查一下网卡类型。

* 执行 `lspci -k` 查看 Network controller 这一项。

```
02:00.0 Network controller: Broadcom Limited BCM43228 802.11a/b/g/n
Subsystem: Foxconn International, Inc. BCM43228 802.11a/b/g/n
Kernel driver in use: bcma-pci-bridge
Kernel moudles: bcma
```

* 检查 `ip link` 输出结果中有没有 `wlan0` 或 `wlps21` 一类的设备。如果有的话，只需要执行 `ip link set <设备名> up` 即可，无需再额外安装驱动程序。

* 如果没有，即需要检查内核的固件信息 `dmesg | grep firmware`，得到更多提示

```
b43-phy0 ERROR: You must go to http://wireless.kernel.or/en/users/Drivers/b43#devicefirmware and download the correct firmware for this driver version.
```

* 如果没有这样的信息，可以先尝试执行 `iwlwifi` 一类命令，然后查找对应的错误信息 `dmesg | grep iwlwifi`
* 获得提示信息后，例如上面的网站，可以访问得到进一步的信息。此处根据上述 `ERROR` 中的网站给出的指导，执行 `lspci -nn -d 14e4:` 获得详细的硬件型号，并下载[对应的驱动](https://aur.archlinux.org/packages/b43-firmware/)。关于博通无线网卡，可以查阅 [Wiki](https://wiki.archlinux.org/index.php/Broadcom_wireless) 页面 获得更多信息

## 下载并安装驱动

* 没网络怎么下载 ❓ 可以下载至安装U盘的根目录，安装时切换至 `/run/archiso/bootmnt`，即可看到下载的内容，将其拷贝至 `/root` 解压。
* 如何来安装驱动 ❓ [更多参考](https://www.cnblogs.com/clouds008/p/7460928.html) 在上一步解压完成后，会得到一个 `.o` 结尾的问题，执行 `b43-fwcutter -w /lib/firmware xxx.o` 即可完成安装。
* 安装后怎么启动 ❓ [更多参考](http://linuxwireless.sipsolutions.net/en/users/Drivers/b43/) 卸载之前的驱动，然后再加载新安装的驱动。
  * 卸载旧驱动（如果你知道你正在使用的驱动名称的话，只需执行对应的命令即可）
    ```bash
    modprobe -r b43 bcma
    modprobe -r brcmsmac bcma
    modprobe -r wl
    ```
  * 加载新驱动（只需执行对应的驱动命令即可）
    ```bash
    modprobe b43
    modprobe brcmsmac
    modprobe wl
    ```

* 安装完成后可以通过 `ip link` 查看是否有新增加的网络接口信息。

## 安装完成后的联网过程

> 此部分内容主要参考了 [在命令行中管理 Wifi 连接](https://linux.cn/article-4015-1.html)

* 通过 `ip link set <设备名> up` 启动接口服务。
* 可以通过 `iw dev <设备名> scan | less` 扫描附近网络。
* 联网（方式 1）无加密网络
    ```bash
    iw dev <设备名> connect <网络 SSID>
    ```
* 联网（方式 2）WEP 加密
    ```bash
    iw dev <设备名> connect <网络 SSID> key 0:<WEP密钥>
    ```

* 联网（方式 3）WPA/WPA2 加密（现在最常见）
    ```txt
    network={
        ssid="<网络 SSID>"
        psk="<密码>"
        priority=1
    }
    ```
  * `vim /etc/wpasupplicant/wpa_supplicant.conf` 编辑文件（其实放在别的位置也行），增加以上内容（注意包含引号）
  * `wpa_supplicant -i <设备名> -c /etc/wpa_supplicant/wpa_supplicant.conf &` 后台启动网络连接

* 获取 IP：`dhcpcd <设备名>` 自动获取 IP 地址
* 测试网络：`ping hiwangzi.com` 测试网络连接是否正常~
