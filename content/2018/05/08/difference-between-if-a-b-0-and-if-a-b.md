---
title: "「译」if(a - b < 0) 与 if(a < b) 的区别"
date: 2018-05-08T00:00:00+08:00
draft: false
summary: "一个针对此问题 Stack Overflow 上的回答。"
tags: ["Java"]
---

## 目录

- [问题](#问题)
- [赞同最多的回答](#赞同最多的回答)

> 原文地址：[java - Difference between if (a - b < 0) and if (a < b) - Stack Overflow](https://stackoverflow.com/questions/33147339/difference-between-if-a-b-0-and-if-a-b)

## 问题

在阅读 Java 的 `ArrayList` 源代码时，我注意到在 `if` 语句中的一些比较语句。

在 Java 7 之中，[`grow(int)`](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/7-b147/java/util/ArrayList.java#ArrayList.grow%28int%29) 方法使用了如下代码：

```java
if (newCapacity - minCapacity < 0)
    newCapacity = minCapacity;
```

在 Java 6 中，没有 `grow` 方法。但方法 [`ensureCapacity(int)`](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/6-b27/java/util/ArrayList.java#ArrayList.ensureCapacity%28int%29) 却使用如下方式比较数值：

```java
if (newCapacity < minCapacity)
    newCapacity = minCapacity;
```

变更比较语句写法的原因是什么呢？为了性能还是说只是代码风格的变化？

我可以理解，与 0 作比较速度更快，但同时却需多执行一个减法，这不是额外的开销吗？同时对于编译后的字节码来说，做差比较需要涉及两个指令（`ISUB` 和 `IF_ICMPGE`），而直接比较只需要涉及 `IFGE` 一条指令。

## 赞同最多的回答

`a < b` 与 `a - b < 0` 可以表示不同的含义。

考虑如下场景：

```java
int a = Integer.MAX_VALUE;
int b = Integer.MIN_VALUE;
if (a < b) {
    System.out.println("a < b");
}
if (a - b < 0) {
    System.out.println("a - b < 0");
}
```

执行结果是：只会打印 `a - b < 0`。因为 `a < b` 显然是 false，但 `a - b` 产生了溢出，结果是 `-1`，因此结果为 true。

* 译者注：
    ```java
    int max = Integer.MAX_VALUE; //  2147483647
    int min = Integer.MIN_VALUE; // -2147483648
    ```

说回问题本身，我们假设数组长度本身已经非常接近于 `Integer.MAX_VALUE` 来看一看。`ArrayList` 之中的代码如下：

```java
// 译者注：可以查看问题中的第一个链接 grow(int) 方法
int oldCapacity = elementData.length;
int newCapacity = oldCapacity + (oldCapacity >> 1);
if (newCapacity - minCapacity < 0)
    newCapacity = minCapacity;
if (newCapacity - MAX_ARRAY_SIZE > 0)
    newCapacity = hugeCapacity(minCapacity);
```

`oldCapacity` 非常接近 `Integer.MAX_VALUE`，因此 `newCapacity`（即`oldCapacity + 0.5 * oldCapacity`）有可能产生溢出，假设溢出后值为 `Integer.MIN_VALUE` (负值)。然后，减去 `minCapacity` 又**下溢**变为正值（译者注：假设参数`minCapacity`为正值）。

因此第一个 `if` 块中的代码将不会被执行。但假设代码条件写作 `if (newCapacity < minCapacity)`，结果将会是 `true`（因为 `newCapacity` 是负值），这将导致 `newCapacity` 被强行指定为 `minCapacity` 而没有考虑 `minCapacity`。

这个溢出问题将会被下一个 `if` 语句所处理。当 `newCapacity` 溢出后，此 `if` 的结果为 `true`：`MAX_ARRAY_SIZE` 为 `Integer.MAX_VALUE - 8`，所以 `Integer.MIN_VALUE - (Integer.MAX_VALUE - 8) > 0` 为 `true`。因此 `newCapacity` 可以被正确的处理：`hugeCapacity` 方法将返回 `MAX_ARRAY_SIZE` 或 `Integer.MAX_VALUE`。

注意：这就是对应 Java 代码中注释 `// overflow-conscious` 的含义。
