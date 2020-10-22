---
title: "截图 | 识别二维码"
date: 2020-05-21T12:05:27+08:00
draft: false
summary: "macOS中截图后识别二维码"
tags: ["Life"]
---

### 目的

* 不想掏出手机扫描二维码
* 不想保存成文件，再使用解析工具（太麻烦

### 目标

* 截图后可以自动识别二维码内容

### 使用说明

* 需要安装：`pngpaste`, `zbar` (使用 homebrew 安装: `brew install pngpaste zbar`）
* 使用截图软件，保存至剪贴板后，执行以下命令

```
pngpaste - | zbarimg -q PNG:- | awk -F 'QR-Code:' '{print $2}'
```

### 演示

![操作演示](../resources/demo.gif)

* 我使用的截图软件 [Xnip](https://apps.apple.com/us/app/xnip-screenshot-annotation/id1221250572)
* 此处我为上述命令定义了别名 `dqr`
* 演示中的二维码（你也可以试一试
  ![本站二维码](../resources/qrcode-hiwangzi.com.png)
