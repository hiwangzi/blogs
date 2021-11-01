---
title: 【Java 核心笔记】04. equals、hashcode、toString方法
date: 2017-10-07T21:00:00+08:00
draft: false
summary: ""
tags: ["Java", "Java 核心笔记"]
---

## equals

* equals 方法示例

```java
// 代码来自《Java核心技术 卷I》P167
// 父类
public class Employee{
    ...
    public boolean equals(Object otherObject){
        // a quick test to see if the objects are identical
        if(this == otherObject) return true;

        // must return false if the explicit parameter is null
        if(otherObject == null) return false;

        // if the classes don't match, they can't be equal
        // 笔者注：子类通过super.equals方法调用到此处时，getClass()的结果是子类
        if(getClass() != otherObject.getClass())
            return false;

        // now we know otherObject is a non-null Employee
        Employee other = (Employee) otherObject;

        // test whether the fields hava identicial values
        // 笔者注：此处使用Objects.equals方法是为了防备name或hireDay可能为null的情况
        return Objects.equals(name, other.name)
            && salary == other.salary
            && Objects.equals(hireDate, other.hireDate);
    }
}
// 子类
// 先调用超类的equals，如果返回false，对象则不可能相等
// 如果父类中的域都相等，再比较子类的实例域
public class Manager extends Employee{
    ...
    public boolean equals(Object otherObject){
        if(!super.equals(otherObject)) return false;
        // super.equals checked that this and otherObject belong to the same class
        Manager other = (Manager) otherObject;
        return bonus == other.bonus;
    }
}
```

* Java 语言规范要求 equals 方法具有以下特性：
    * 自反性：x.equals(x) 应当返回 true
    * 对称性：x.equals(y) 与 y.equals(x) 返回应当相同
    * 传递性：如果 x.equals(y) 返回 true，且 y.equals(z) 也返回 true，则 x.equals(z) 也应返回 true
    * 一致性：如果 x 与 y 引用的对象没有发生变化，则 x.eqauls(y) 也不应变化
    * 对于任意的非空引用 x，x.equals(null) 应当返回 false

* 在上面的例子中，如果发现类型不一致，就返回 false。但同时也有许多程序员喜欢采用以下代码进行检测 ```if(!(otherObject instanceof Employee)) return false;``` 但这样没有解决 otherObject 是子类的情况(父类对象.eqaules(子类对象))下的比较问题。

* 关于 getClass 与 instanceof 两种检测方法：
    * 如果子类能够拥有自己的相等概念，则对称性需求将强制采用 getClass 进行检测。
    * 如果由超类决定相等的概念，那么就可以使用 instanceof 进行检测，这样可以在不同子类的对象之间进行相等的比较。

* 编写完美的 equals 方法的建议：
    1. 显式参数命名为 otherObject，稍后需要将它转换为另一个叫做 other 的变量。
    2. 检测 this 与 otherObject 是否引用同一个对象：```return this == otherObject;```
    3. 检测 otherObject 是否为 null，是则返回 false。
    4. 比较 this 与 otherObject 是否属于同一个类：
        * 如果 equals 的语义在每个子类中有所改变，就使用 getClass 检测：```return getClass() != otherObject.getClass();```
        * 如果所有的子类都拥有统一的语义，就使用 instanceof 检测：```return (!(otherObject instanceof ClassName));```
    5. 将 otherObject 转换为相应的类类型变量：```ClassName other = (ClassName) otherObject```
    6. 对所有需要比较的域进行比较。使用 == 比较基本类型域，使用 equals 比较对象域。如果所有的域都匹配，则返回 true，否则返回false。
        ```java
        return field1 == other.field1
            && Objects.equals(field2, other.field2)
            && ...;
        ```
        如果在子类中重新定义 `equals`，就要在其中包含调用 `super.equals(other)`。


## hashCode 方法

* 散列码（hash code）是由对象导出的一个整形值（可以是负数）。其是没有规律的，如果x与y是两个不同的对象，则x.hashCode()与y.hashCode()基本上不会相同。
* hashCode 方法定义在 Object 类中，因此每个对象都有一个默认的散列码方法，其返回结果是对象的存储地址。
    * 一个例子：
        ```java
        String string1 = "hiwangzi";
        StringBuilder stringBuilder1 = new StringBuilder(string1);
        System.out.println(string1.hashCode() + " " + stringBuilder1.hashCode());
  
        String string2 = new String("hiwangzi");
        StringBuilder stringBuilder2 = new StringBuilder(string2);
        System.out.println(string2.hashCode() + " " + stringBuilder2.hashCode());
        ```
    * 输出结果：
        ```
        -1232882509 1975012498
        -1232882509 1808253012
        ```

      可以看到，String对象的散列码是相同的，这是因为字符串的散列码是由内容导出的;而StringBuffer对象散列码不同，这是因为StringBuffer类没有定义hashCode()方法，它的散列码是由默认的Object类的默认hashCode()方法导出的对象存储地址。
* 如果重新定义 equals 方法，就必须重新定义 hashCode 方法，以便于可以将对象插入到散列表中。
* 可以调用 Objects.hash 方法并提供多个参数得到散列码（这种做法比较好）：
    ```java
    public int hashCode(){
        return Objects.hash(name, salary, hireDay);
    }
    ```
* equals 与 hashCode 定义必须一致，即 `x.equals(y)` 与 `x.hashCode() == y.hashCode()` 结果一致。

## toString 方法

* 绝大多数的 toString 方法都遵循这样的格式：类的名字，随后一对方括号括起来的域值。
    ```java
    public String toString(){
        return getClass().getName()
            + "[name=" + name
            + ",salary=" + salary
            + ",hireDay=" + hireDay
            + "]";
    }
    ```