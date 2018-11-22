---
title: java学习
tags: java
categories: java
archives: java
photos:
  - 'http://oz2tkq0zj.bkt.clouddn.com/17-11-9/52323298.jpg'
date: 2018-10-30 19:42:08
---

##  java基础语法
1.  对象：对象是类的一个实例，有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
2.  类：类是一个模板，它描述一类对象的行为和状态。
3.  方法：方法就是行为，一个类可以有很多方法。逻辑运算、数据修改以及所有动作都是在方法中完成的。
4.  实例变量：每个对象都有独特的实例变量，对象的状态由这些实例变量的值决定。

### 第一个Java程序
```
public class HelloWorld {
  public static void main(String []args){
    System.out.println("Hello,world!");
  }
}
```

##  对象和类
类可以看成创建对象的模板。
```
public  class Dog {
  String  breed;
  int age;
  String  color;
  void  barking(){

  }
}
```
一个类可以包含以下类型变量
- 局部变量：在方法、构造方法或者语句块中定义的变量。
- 成员变量：定义在类中，方法体之外的变量。
- 类变量:声明为static类型。

创建对象
1.  声明一个对象
2.  实例化，使用关键字new创建一个对象。
3.  初始化，使用new创建对象是，会调用构造方法初始化对象。
```
public class Puppy {
  public Puppy(String name) {
    System.out.println("我的小狗的名字是" + name);
  }
  public static void main(String []args) {
    Puppy myPuppy = new Puppy("小黄")
  }
}
```

### 访问实例变量和方法

`ObjectReference = new Constructor`,
`ObjectReference.variablename`,
`ObjectReference.methodName()`

##  基本数据类型

- 内置数据类型
- 引用数据类型

### 内置数据类型

1.  byte：8位，有符号，最小值`-128（-2^7）`，最大值是`127（2^7-1）`。
2.  short：16位，有符号，最小值`-32768（-2^15）`，最大值是32767`（2^15-1）`。
3.  int：32位，有符号，最小值是`-2,147,483,648（-2^31）`，最大值是 `2,147,483,647（2^31 - 1）`。
4.  long：64位，最小值是`9,223,372,036,854,775,808（-2^63）`，最大值是 `9,223,372,036,854,775,807（2^63 -1）`。
5.  float：单精确，32位。
6.  double：双精确，64位。
7.  boolean：true,false,默认值是false。
8.  char：单一的16位Unicode字符。

### 引用类型

引用类型指向一个对象，指向对象的变量是引用变量。对象，数值都是引用数据类型，默认值位null。例如：`Site site = new Site("Runoob")`。

### Java常量

在Java中使用`final`关键字修饰变量，声明方式为`final double PI = 3.14.159`。
常用名常用小写表示变量，大写表示常量。

### 自动类型转换

**整型、常量、字符型**可以混合运算，不同类型先转化为同一类型，然后进行运算，转换从低级到高级。

###  变量类型

- 类变量：独立于方法之外的变量，用`static`修饰。
- 实例变量：独立于方法之外的变量，**不要**`static`修饰。
- 局部变量：类的方法之中的变量。
  
```
public class Variable {
  static  int a = 0;  //类变量
  String str = "hello,world"; 实例变量
  public void method(){
    int i = 1;  //局部变量
  }
}
```

### 循环
1.  for循环
2.  while循环
3.  do while循环
4.  foreach