---
title: 【Java 核心笔记】04.06. UML
date: 2017-10-14T18:01:00+08:00
draft: false
summary: ""
tags: ["Java", "Java 核心笔记"]
---

## 类间关系

* 依赖(dependence)（uses-a）：例如Order对象使用Account对象查看账户的信用状态
* 聚合(aggregation)（has-a）：例如Order对象包含了一些Item对象
* 继承(inheritance)（is-a）

## 对应的 UML 符号

![UML符号](./resources/uml-notation-for-class-relationships.png)

* 有些方法学家不喜欢聚合这个概念，而更加喜欢「关联(association)」这个术语。但「has-a」更加形象，同时「关联」的 UML 符号不易区分。

* 以下是一个UML类图的示例：

  ![UML类图示例](./resources/class-diagram-example.png)
