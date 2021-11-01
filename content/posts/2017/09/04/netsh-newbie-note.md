---
title: 「cmd」与「网卡」—— netsh 命令
date: 2017-09-04T19:13:00+08:00
draft: false
summary: ""
tags: ["Windows"]
---

## 1. 通过命令提示符（cmd）命令连接 Wi-Fi

### 1.1 连接曾经连接过的 Wi-Fi
```bat
:: 查看配置的列表（::表示注释）
netsh wlan show profile
:: 连接
netsh wlan connect ssid=你的SSID名字(简单可以理解为Wi-Fi名) name=你的配置名字
```

### 1.2 连接从未连接过的 Wi-Fi
```bat
:: 先增加一项 Wi-Fi 配置，注意要在配置文件所在目录执行
netsh wlan add profile filename="你的配置.xml"
:: 查看配置的列表，检查是否添加成功
netsh wlan show profile
:: 连接
netsh wlan connect ssid=你的SSID名字 name=你的配置名字
```

#### 示例配置文件
```xml
<?xml version="1.0"?>
<WLANProfile xmlns="http://www.microsoft.com/networking/WLAN/profile/v1">
    <name>你的配置名字（与SSID名字相同即可）</name>
    <SSIDConfig>
        <SSID>
            <name>你的SSID名字</name>
        </SSID>
    </SSIDConfig>
    <connectionType>ESS</connectionType>
    <connectionMode>auto</connectionMode>
    <MSM>
        <security>
            <authEncryption>
                <authentication>WPA2PSK</authentication>
                <encryption>AES</encryption>
                <useOneX>false</useOneX>
            </authEncryption>
            <sharedKey>
                <keyType>passPhrase</keyType>
                <protected>false</protected>
                <keyMaterial>你的WiFi密码</keyMaterial>
            </sharedKey>
        </security>
    </MSM>
    <MacRandomization xmlns="http://www.microsoft.com/networking/WLAN/profile/v3">
        <enableRandomization>false</enableRandomization>
    </MacRandomization>
</WLANProfile>
```

## 2. 查看连接过的 Wi-Fi 密码
```bat
:: 查看特定
netsh wlan show profile 配置名称 key=clear

:: 查看所有
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profiles') do  @echo %j | findstr -i -v echo | netsh wlan show profiles %j key=clear
```

## 3. 查看、禁用、启用网络接口（网卡）
```bat
:: 查看网络接口的列表
netsh interface show interface
:: 禁用网络接口
netsh interface set interface 接口名称 disabled
:: 启用网络接口
netsh interface set interface 接口名称 enabled
```