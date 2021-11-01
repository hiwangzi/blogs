---
title: 命令行计算 RSA, Base64, Hash
date: 2017-11-20T15:05:00+08:00
draft: false
summary: "OpenSSL 是个好工具 👍"
tags: ["Linux"]
---

## 基础

```bash
# -n 可以去掉换行符
echo -n '777777'
```

## RSA算法

* 加密
    ```bash
    # 利用管道命令传递字符串加密
    echo -n '777777' | openssl rsautl -encrypt -pubin -inkey public_key.pem > message.encrypted

    # （或）利用文件传递字符串加密
    echo -n '777777' > message.txt

    openssl rsautl -encrypt -pubin -inkey public_key.pem -in message.txt > message.encrypted
    ```

* 解密
    ```bash
    openssl rsautl -decrypt -inkey private_key.pem -in message.encrypted -out message.decrypted
    ```

## Base64

* 编码
    ```bash
    openssl enc -base64 -e -in message.txt > message.base64e
    ```

* 解码
    ```bash
    openssl enc -base64 -d -in message.base64 > message.base64d
    ```

## Hash

* MD5

    ```bash
    echo -n '777777' | md5sum
    ```