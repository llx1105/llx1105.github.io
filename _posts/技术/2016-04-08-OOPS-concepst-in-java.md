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

```
Boy is eating
```

一些重写的规则：

  1. 重写的方法的参数，顺序，返回类型等必须一致.

  2. 重写的方法不能比父类更严格，例如父类的方法是public的则重写的不能是protected或者private。反之则可以。

  3. private, static以及final methods 不能被重写。

  4. 重写的方法的绑定发生在run-time,即 dynamic binding。

  5. 如果一个类继承或者实现的abstract 或者interface 那么必须重写所有的方法，除非它自己是一个abstract class。

  6. super 调用父方法。

##Difference between method Overloading and Overriding in java

###动态绑定和静态绑定
  在Java中，当你调用一个方法时，可能会在编译时期（compile time）解析（resolve），也可能实在运行时期（runtime）解析，这全取决于到底是一个静态方法（static method）还是一个虚方法（virtual method）。

  如果是在编译时期解析，那么就称之为静态绑定（static binding），如果方法的调用是在运行时期解析，那就是动态绑定（dynamic binding）或者延迟绑定（late binding）。
Java是一门面向对象的编程语言，优势就在于支持多态（Polymorphism）。

  多态使得父类型的引用变量可以引用子类型的对象。如果调用子类型对象的一个虚方法（非private,final or static），编译器将无法找到真正需要调用的方法，因为它可能是定义在父类型中的方法，也可能是在子类型中被重写（override）的方法，这种情形，只能在运行时进行解析，因为只有在运行时期，才能明确具体的对象到底是什么。

  这也是我们俗称的运行时或动态绑定（runtime or dynamic binding）。另一方面，private static和final方法将在编译时解析，因为编译器知道它们不能被重写，所有可能的方法都被定义在了一个类中，这些方法只能通过此类的引用变量进行调用。

  这叫做静态绑定或编译时绑定（static or compile time binding）。所有的private,static和final方法都通过静态绑定进行解析。这两个概念的关系，与“方法重载”（overloading，静态绑定）和“方法重写”（overriding，动态绑定）类似。动态绑定只有在重写可能存在时才会用到，而重载的方法在编译时期即可确定（这是因为它们总是定义在同一个类里面）

Overloading vs Overriding in Java：

1. 重载发生在编译期而重写发生在运行期.重载是静态绑定的而重写是动态绑定，可以说重写实现了多态。

2. static 方法可以被重载，意味着一个类可以拥有多个相同名字的静态方法，但是静态方法并不能被重写，虽然形式上可以重写，但是然并卵。

3. 重载是在同一个类中，而重写发生在子类。

4. private和final methods可以被重载但是不能被重写。

##Constructors

构造方法是一个比较特殊的方法，它没有返回类型，在new 一个对象的时候会调用，如果你没有定义，会调用一个默认的午参构造方法。

需要注意的一点是，如果你定义了一个构造方法，系统是不会再给你定义默认的构造方法了，像下面这样是会报错的：

```
package beginnersbook.com;
public class Demo
{
  private int rollNum;
  We are not defining a no-arg constructor here

    Demo(int rnum)
    {
      rollNum = rollNum+ rnum;
    }
  //Getter and Setter methods
}
class TestDemo{
  public static void main(String args[])
  {
    //This statement would call no-arg constructor
    Demo obj = new Demo();
  }
}
```
output:

```
Exception in thread "main" java.lang.Error: Unresolved compilation 
problem:The constructor Demo() is undefined
```

另外，构造方法只能重载，不能重写，因为子类怎么重写，名字都不一样了。

interface没有构造方法。

##private constructor
私有构造方法可以用来创建单例。单例模式可以保证系统中一个类只有一个实例。

example:

```
package beginnersbook.com;
public class SingleTonClass {
  Static Class Reference
    private static SingleTonClass obj=null;
  private SingleTonClass(){
    /*Private Constructor will prevent 
     * the instantiation of this class directly*/
  }
  public static SingleTonClass objectCreationMethod(){
    /*This logic will ensure that no more than
     * one object can be created at a time */
    if(obj==null){
      obj= new SingleTonClass();
    }
    return obj;
  }
  public void display(){
    System.out.println("Singleton class Example");
  }
  public static void main(String args[]){
    //Object cannot be created directly due to private constructor 
    //This way it is forced to create object via our method where
    //we have logic for only one object creation
    SingleTonClass myobject= SingleTonClass.objectCreationMethod();
    myobject.display();
  }
}
```
