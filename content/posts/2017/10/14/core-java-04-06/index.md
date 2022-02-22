---
title: 【Java 核心笔记】04.06. UML、类的一些要点补充
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

## 类的一些要点补充

* 使用类的方法时，会隐式包含一个参数——方法名前的类对象（方法之中使用`this`调用），这被称为隐式(implicit)参数。其他参数称为显式(explicit)参数。
* [方法接收参数的形式是按值调用(call by value)](/posts/2017/09/27/core-java-03/#3-使用方法交换变量值)。
* 可以通过「方法的签名(signature)」来完整地描述一个方法（方法名+参数类型，不包含返回类型），例如`String`类的`indexOf`方法
    * `indexOf(int)`
    * `indexOf(int, int)`
    * `indexOf(String)`
    * `indexOf(String, int)`
* 默认域的初始化
    * 如果没有初始化类中的域，将会被自动初始化为默认值（0、false或null）。
    * 但局部变量不可，必须明确地初始化方法中的局部变量。
* 类中代码执行顺序原则：
    * 静态优先（静态域、静态代码块）
    * 对于非静态，顺序为：域(初始化语句)、构造代码块（初始化块）、构造方法
    * 总结：**父类静态元素 -> 子类静态元素 -> 父类非静态元素 -> 子类非静态元素**
* 装箱和拆箱是编译器认可的，而不是虚拟机。编译器在生成类的字节码时，插入必要的方法调用。虚拟机只是执行这些字节码。

### 包

* 从编译器的角度来看，嵌套的包之间没有任何关系。
* `import java.time.*` 对代码的大小无负面影响，但会降低人可读性。
* 只能使用星号(`*`)导入 **一个** 包，而不能使用`import java.*`或`import java.*.*`导入以`java`为前缀的所有包。
* 在包中定位类是编译器的工作，字节码中使用完整的包名引用其他类。
* `import static java.lang.System.*`这样的写法可以导入`System`类的静态方法和静态域，可以直接使用而无需加类名前缀。