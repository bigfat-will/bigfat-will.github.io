---
layout:     post
title:      "Java设计模式/单例模式"
subtitle:   ""
date:       2017-08-01
author:     "Will"
header-img: "img/java/java.png"
catalog: true
tags:
    - Java设计模式
---

> “walk beside you ”


## 前言 

　　**单例模式**作为对象的创建模式，单例模式确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。这个类称为单例类。
　　
---

## 正文

   单例模式的特点：
*  单例类只能有一个实例
*  单例类必须自己创建自己的唯一实例
*  单例类必须给所有其他对象提供这一实例

## 饿汉式单例
　　 饿汉式单例在类加载初始化时就创建好一个静态的对象供外部使用，除非系统重启，这个对象不会改变，所以本身就是线程安全的。
```
   // 饿汉式单例
   public class Singleton {
       // 私有构造
       private Singleton() {}
       private static Singleton1 single = new Singleton1();
       // 静态工厂方法
       public static Singleton1 getInstance() {
           return single;
       }
   }
```
　　
---

## 懒汉式单例

　　 与饿汉不同懒汉采用延迟加载方式，只有使用了才创建对象

```
public class Singleton {
    // 私有构造
    private Singleton() {}
    private static Singleton single = null;
    // 双重检查
    public static Singleton getInstance() {
        if (single == null) {
            synchronized (this) {
                if (single == null) {
                    single = new Singleton4();
                }
            }
        }
        return single;
    }
}
```
　　
---

## 静态内部类实现

　　 （静态内部类在调用的时候初始化）静态内部类虽然保证了单例在多线程并发下的线程安全性，但是在遇到序列化对象时，默认的方式运行得到的结果就是多例的。需注意。

```
public class Singleton {
    // 私有构造
    private Singleton() {}
    // 静态内部类
    private static class InnerObject{
        private static Singleton single = new Singleton();
    }
    public static Singleton getInstance() {
        return InnerObject.single;
    }
}
```
　　
---

## 静态代码块实现

```
public class Singleton {
    // 私有构造
    private Singleton() {}
    private static Singleton single = null;
    // 静态代码块
    static{
        single = new Singleton();
    }
    public static Singleton getInstance() {
        return single;
    }
}
```
　　
---

## 枚举

```
public enum Singleton{
    INSTANCE;
    public void getInstance(){
        System.out.println("Something");
    }
}
```
---



