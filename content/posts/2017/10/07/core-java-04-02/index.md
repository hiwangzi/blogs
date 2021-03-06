---
title: 【Java 核心笔记】04.02. Override 的补充
date: 2017-10-07T17:31:00+08:00
draft: false
summary: ""
tags: ["Java", "Java 核心笔记"]
---

## 方法的覆写

* 当子类定义了与父类方法名称相同、参数的类型及个数、**返回值相同**的方法时，就被称为方法的覆写。

* 被覆写的方法不能拥有比父类方法更严格的访问权限。`private < default < protected < public`
    * 如果父类中的方法使用了 `private` 声明，而子类中同样的方法使用了 `public` 声明，这样属于覆写么？不属于。
      ```java
      class A {
          public void fun() {
              print();
          }
          /*①*/ void print() {
              System.out.println("父类中的 print() 方法");
          }
      }
      class B extends A {
          /*②*/ void print() { // 覆写的是 print() 方法
              System.out.println("子类中的 print() 方法");
          }
      }
      public class OverrideDemo {
          public static void main(String [] args) {
              B b = new B();
              b.print();
              b.fun();
          }
      }
      // ① public;  ② public  属于重写，输出结果为“子类；子类”
      // ① public;  ② private 不属于重写，因为不符合重写原则，无法编译通过
      // ① private; ② public  不属于重写，因为如果父类中方法使用了 private 声明，
      //                        那这个方法对于子类而言是不可见的，即使它符合了覆写的访问限制要求，
      //                        但仍然不能够动态的实现重写的效果，输出结果为“子类；父类”。
      //                        此时子类中的方法相当于子类新定义了一个方法，与父类中的同名方法无关
      ```

#### 关于 `this.方法()` 与 `super.方法()`

* 使用 `this.方法()` 会首先查找本类中是否存在有要调用的方法名称，如果存在则直接调用，如果不存在则查找父类中是否具有此方法，如果有就调用，如果没有，则会出现编译时错误。
* 使用 `super.方法()` 明确的表示调用的方法不是子类中的方法（不查找子类），而直接调用父类中的指定方法。

### 属性的覆盖（了解）

* 子类定义了和父类完全相同的属性名称时，就称为属性的覆盖。
* 与方法的覆盖表现不同，实际上不能实现「动态多态」

```java
public class VarOverrideDemo {
    public static void main(String [] args) {
        B b = new B();
        b.print();  // Hello
                    // 100
        
        System.out.println(c.str);      // C
        System.out.println(d.str);      // D
        System.out.println(c.getStr()); // D
        System.out.println(d.getStr()); // D
    }
}

class A {
    String info = "Hello";
}
class B extends A {
    int info = 100; // 名称与父类中变量相同
    public void print(){
        System.out.println(super.info);
        System.out.println(this.info);
    }
}

class C {
    String str = "C";

    public String getStr() {
        return str;
    }
}
class D extends C {
    String str = "D";

    // 即使不加Override注解，也能实现动态绑定
    // @Override
    public String getStr() {
        return str;
    }
}
```

但其实在实际开发中，这种特性意义不是很大，因为绝大多数情况，属性都是被封装的，而被限制为 `private` 后，对于子类而言不可见，因此不会相互影响。

## 关于 `this` 与 `super` 的小总结

|区别|this|super|
|---|----|------|
|功能|调用本类构造、本类方法、本类属性|子类调用父类构造、父类方法、父类属性|
|形式|先查找本类中时候存在指定的调用结构，有则调用，无则调用父类定义|不查找子类，直接调用父类调用结构|
|特殊|表示本类的当前对象||

建议在开发中，加上 `this.` 或者 `super.`，这样便于区分。