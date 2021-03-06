---
title: Python 3 基础总结
date: 2017-08-31T18:20:00+08:00
draft: false
summary: ""
tags: ["Python"]
---

## 基础
* Python 的语法比较简单，采用缩进方式
* 以 ```#``` 开头的语句是注释
* 当语句以冒号 ```:``` 结尾时，缩进的语句视为代码块
* 没有规定缩进是几个空格还是 Tab，但按照约定俗成的惯例，应该始终坚持使用4个空格的缩进
* 确保不混用 Tab 和空格
* Python 程序是大小写敏感的

## 输入输出
* ```print()```
* ```input()```

## 数据类型
* 整数
* 浮点数
    * 对于很大或很小的浮点数，用科学计数法表示，把10用e替代，例: 1.23x10⁹就是1.23e9, 0.000012可以写成1.2e-5
* 字符串
    * 可以用```r''```表示```''```内部的字符串默认不转义
* 布尔值
* 空值
    * 一个特殊的值，用 ```None``` 表示。
* 常量
    * 变量名全大写表示常量, 事实上仍然是变量

## 字符串与编码
由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
* 第一行注释是为了告诉 Linux/OS X系统，这是一个 Python 可执行程序，Windows系统会忽略这个注释；
* 第二行注释是为了告诉 Python 解释器，按照 UTF-8 编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

## 格式化
```
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
>>> '%2d-%02d' % (3, 1)
' 3-01'
>>> '%.2f' % 3.1415926
'3.14'
```

## list, tuple 与 dict, set
* list: 一种有序的集合, 可以随时添加和删除其中的元素, 函数: ```append(var)```, ```insert(index, var)```, ```pop()```, ```pop(index)```
    ```python
    classmates = ['Michael', 'Bob', 'Tracy']
    ```
* tuple: 类似于 list, 但初始化后不能修改
    ```python
    classmates = ('Michael', 'Bob', 'Tracy')
    ```
* dict: 字典, 使用键-值(key-value)存储, 具有极快的查找速度, 函数: ```get(key)```, ```pop(key)```
    ```python
    d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
    ```
* set: 类似于 dict, 但不存储 value, 只存储 key(不可重复), 函数: ```add(key)```, ```remove(key)```
    ```python
    # 要创建一个set，需要提供一个list作为输入集合
    s = set([1, 2, 3])
    ```
