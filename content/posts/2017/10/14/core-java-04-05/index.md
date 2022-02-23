---
title: 【Java 核心笔记】04.05. 内部类、lambda
date: 2017-10-14T16:44:00+08:00
draft: false
summary: ""
tags: ["Java", "Java 核心笔记"]
---

在类的内部还可以定义另一个类。如果在类 `Outter` 的内部再定义一个类 `Inner` ，此时类 `Inner` 就称为内部类，而类 `Outter` 则称为外部类。
内部类可声明为 `public` 或 `private`。当内部类声明为 `public` 或 `private` 时，对其访问的限制与成员变量和成员方法完全相同。

```java
class Outer {                               // 定义外部类
    private String info = "hello world";    // 定义外部类的私有属性
    class Inner {                           // 定义内部类
        public void print(){                // 定义内部类的方法
            System.out.println(info);       // 直接访问外部类的私有属性
        }
    }
    public void fun() {                     // 定义外部类的方法
        new Inner().print();                // 通过内部类的实例化对象调用方法
    }
}
public class InnerClassDemo01 {
    public static void main(String args[]){
        new Outer().fun();                  // 调用外部类的fun()方法
    }
}

// 以上程序中，Inner 类作为 Outter 的内部类存在，
// 并且在外部类的 fun() 方法之中直接实例化内部类的对象并调用 print() 方法。
```

内部类存在的特点：
* 缺点：破坏了程序的结构
* 优点：可以方便的访问外部类中的私有属性

四类内部类：
* 成员内部类
* 静态内部类
* 局部内部类
* 匿名内部类

## 在其他类中调用内部类（成员内部类）

一个内部类除了可以通过外部类访问，也可以直接在其他类中调用（这里指的是在声明了外部类对象后，通过外部类对象调用内部类构造方法来实例化一个内部类对象），调用的格式如下：

```java
外部类.内部类 内部类对象 = 外部类实例.new 内部类();
```

```java
class Outer {                               // 定义外部类
    private String info = "hello world";    // 定义外部类的私有属性
    class Inner{                            // 定义内部类
        public void print() {               // 定义内部类的方法
            System.out.println(info);       // 直接访问外部类的私有属性
        }
    }
    public void fun() {                     // 定义外部类的方法
        new Inner().print();                // 通过内部类的实例化对象调用方法
    }
}
public class InnerClassDemo04 {
    public static void main(String args[]) {
        Outer out = new Outer();            // 外部类实例化对象
        Outer.Inner in = out.new Inner();   // 实例化内部类对象
        in.print();                         // 调用内部类的方法
        new Outer().new Inner().print();    // 与上面的代码等效
    }
}
```

## 使用 static 声明内部类（静态内部类）

* 如果一个内部类使用 `static` 关键字声明，则此内部类称为静态内部类（内部类访问控制是 public 的情况下相当于一个外部类，可以直接通过外部类.内部类进行访问）
* 静态内部类是不依赖于外部类的，也就说可以在不创建外部类对象的情况下创建内部类的对象。另外，静态内部类不持有指向外部类对象的引用（可以反编译代码查看）。

```java
class Outer{                                    // 定义外部类
    private static String info = "hello world"; // 定义外部类的私有属性
    static class Inner {                        // 使用static定义内部类为外部类
        public void print() {                   // 定义内部类的方法
            System.out.println(info);           // 直接访问外部类的私有属性
        }
    }
}
public class InnerClassDemo03 {
    public static void main(String args[]) {
        new Outer.Inner().print();
    }
}
```

## 在方法中定义内部类（局部内部类、匿名内部类）

一个内部类可以在任意的位置定义，下面在方法中定义内部类。

```java
class Outer {                                           // 定义外部类
    private String info = "hello world";                // 定义外部类的私有属性
    public void fun(final int temp) {                   // 定义外部类的方法
        class Inner {                                   // 在方法中定义的内部类
            public void print() {                       // 定义内部类的方法
                System.out.println("类中属性：" + info);  // 直接访问外部类的私有属性
                System.out.println("方法中参数：" + temp);
            }
        }
        new Inner().print();                           // 通过内部类的实例化对象调用方法
    }
}
public class InnerClassDemo05{
    public static void main(String args[]){
        new Outer().fun(30) ;                           // 调用外部类的方法
    }
}
```

以上代码还可以写作「匿名内部类」的形式（Java 1.8 后还可以写作 lambda 表达式形式）：

```java
class Outer {                                                // 定义外部类
    private String info = "hello world" ;                   // 定义外部类的私有属性
    public void fun(final int temp) {                        // 定义外部类的方法
        new Object() {
            public void print() {                            // 定义内部类的方法
                System.out.println("类中的属性：" + info);    // 直接访问外部类的私有属性
                System.out.println("方法中的参数：" + temp);
            }
        }.print();
    }
}
```

## lambda 表达式

* lambada 表达式是一个可传递的代码块，重点是延迟执行（deferred execution）。
* 对于***只有一个抽象方法***的接口，需要这种接口的对象时，就可以提供一个 lambda 表达式。这种接口称为函数式接口(functional interface)。
* java.util.function 包中定义了很多非常通用的函数式接口，可以用来保存 lambda 表达式。

### 方法引用

* 使用 `::` 操作符分隔对象(或类)名与方法名，主要有以下3种情况：
    * `object::instanceMethod`，例如：`System.out::println`等价于`x -> System.out.println(x)`
    * `Class::staticMethod`，例如：`Math::pow`等价于`(x,y) -> Math.pow(x, y)`
    * `CLass::instanceMethod`，例如：`String::compareToIgnoreCase`等价于`(x, y) -> x.compareToIgnoreCase(y)`

### 构造器引用

* `Person::new` 指的是 `Person` 的哪一个构造器呢? 这取决于上下文。
* 可以用数组类型建立构造器引用。例如，`int[]::new` 是一个构造器引用，它有一个参数表示数组的长度。这等价于 lambda 表达式 `x -> new int[x]`。
    * 实例：Stream 接口有一个 `toArray` 方法可以返回 Object 数组: `Object[] people = stream.toArray()`，但将 `Person[]::new` 传入 `toArray` 方法，即使用 `Perso[] people = stream.toArray(Person[]::new)` 更佳，可以得到一个正确类型的数组。

### 变量作用域

* lambda 表达式中的3个部分：一个代码块、参数、自由变量的值（非参数且不在代码中定义的变量）
* 因为并发执行多个动作时的安全性，所以在lambda中自由变量的值不允许被改变。在外部可能发生改变同样不合法。
* 在lambda表达式中`this`表示的是创建这个lambda表达式方法的`this`参数。

### 处理 lambda 表达式

#### 常用函数式接口

|函数式接口|参数类型|返回类型|抽象方法名|描述|其他方法|
|---------|-------|-------|--------|---|-------|
|`Runnable`|无|`void`|`run`|作为无参数或无返回值的动作运行||
|`Supplier<T>`|无|`T`|`get`|提供一个`T`类型的值||
|`Consumer<T>`|`T`|`void`|`accept`|处理一个`T`类型的值|`andThen`|
|`BiConsumer<T, U>`|`T, U`|`void`|`accept`|处理`T`和`U`类型的值|`andThen`|
|`Function<T, R>`|`T`|`R`|`apply`|有一个`T`类型参数的函数，返回`R`类型值|`compose`, `andThen`, `identity`|
|`BiFunction<T, U, R>`|`T, U`|`R`|`apply`|有`T`和`U`类型参数的函数，返回`R`类型值|`andThen`|
|`UnaryOperator<T>`|`T`|`T`|`apply`|类型`T`上的一元操作符|`compose`, `andThen`, `identity`|
|`BinaryOperator<T>`|`T`|`T`|`apply`|类型`T`上的二元操作符|`andThen`, `maxBy`, `minBy`|
|`Predicate<T>`|`T`|`boolean`|`test`|判断是否为真|`and`, `or`, `negate`, `isEqual`|
|`BiPredicate<T, U>`|`T, U`|`boolean`|`test`|判断是否为真|`and`, `or`, `negate`|

#### 基本类型的函数式接口

|函数式接口|参数类型|返回类型|抽象方法名|
|---------|-------|-------|--------|
|`IntSupplier`|无|`int`|`getAsInt`|
|`IntConsumer`|`int`|`void`|`accept`|
|`ObjIntConsumer<T>`|`T, int`|`void`|`accept`|
|`IntFunction<T>`|`int`|`T`|`apply`|
|`IntToDoubleFunction<T>`|`int`|`double`|`applyAsDouble`|
|`ToIntFunction<T>`|`T`|`int`|`applyAsInt`|
|`ToIntBiFunction<T, U>`|`T, U`|`int`|`applyAsInt`|
|`IntUnaryOperator`|`int`|`int`|`applyAsInt`|
|`IntBinaryOperator`|`int, int`|`int`|`applyAsInt`|
|`IntPredicate`|`int`|`boolean`|`test`|

* 以`int`为例，其他基本类型同理

## 扩展阅读

* [Java内部类详解 - Matrix海子 - 博客园](https://www.cnblogs.com/dolphin0520/p/3811445.html)
