---
layout: post
title: oops concepst in Java
category: 技术
tags: tech
keywords: Java oop
---

  Java中有很多面向对象的相关的概念和知识，下面边记录边学习和总结一下。

##Method Overloading
  方法的重载：方法重载是指在一个class中允许多个方法拥有相同的名字，甚至构造方法也是。但是方法之间必须有以下区别：

  1. 参数的数量。
  2. 参数的类型。
  3. 参数的顺序。

注意：如果仅仅是参数的返回类型不同，是不可以的.

同时，方法重载实现了静态的多态。

静态的多态是在编译时绑定的，也叫前期绑定。发生在编译期。

##Method Overridding
  在Java中，子类可继承父类中的方法，而不需要重新编写相同的方法。但有时子类并不想原封不动地继承父类的方法，而是想作一定的修改，这就需要采用方法的重写。方法重写又称方法覆盖。
  for example:

  ```
  class Human{
    public void eat()
     {
        System.out.println("Human is eating");
     }
  }

  class Boy extends Human{
    public void eat(){
      System.out.println("Boy is eating");
    }

    public static void main( String args[]) {
      Boy obj = new Boy();
    obj.eat();
    }
  }
```

Output:

Boy is eating

一些重写的规则：
  1. 重写的方法的参数，顺序，返回类型等必须一致.
  2. 重写的方法不能比父类更严格，例如父类的方法是public的则重写的不能是protected或者private。反之则可以。
  3. private, static以及final methods 不能被重写。
  4. 重写的方法的绑定发生在run-time,即 dynamic binding。
  5. 如果一个类继承或者实现的abstract 或者interface 那么必须重写所有的方法，除非它自己是一个abstract class。
  6. super 调用父方法。
