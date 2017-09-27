---
layout:     post
title:      "Java设计模式/观察者模式"
subtitle:   ""
date:       2017-05-15 
author:     "Will"
header-img: "img/java/java.png"
catalog: true
tags:
    - Java设计模式
---

> “walk beside you ”


## 前言 

　　**观察者模式**有叫做发布/订阅(Publish/Subscribe)模式、模型/视图(Model/View)模式、源/监听器(Source/Listener)模式、
从属者(Dependents)模式。
　　
  
 　　观察者模式定义对象间的一种一对多的依赖关系，多个观察者同时监控一个主题。当这个主题发生改变时，所有观察者都会得到通知，
并更新。

---

## 正文
　　在系统中存在这样的场景：当一个模块发生改变时，需要通知其他模块做对应的改变。 
有很多方案可以做到这样的效果，但是为了使系统能够易于复用，应该选择低耦合度的设计方案。
减少对象之间的耦合有利于系统的复用，但是同时设计师需要使这些低耦合度的对象之间能够维持行动的协调一致，保证高度的协作。
观察者模式是满足这一要求的各种设计方案中最重要的一种。


　　观察者模式类图：![观察者模式](/img/java/subject-observer.png)
    
　　观察者模式包含的角色有：
* **抽象主题(Subject)**：创建一个观察者集合，提供添加、删除、通知观察者的接口
```  
   public abstract class Subject {
       private List<Observer> observers = new ArrayList<Observer>();
       public void add(Observer observer){
           System.out.println("添加观察者");
           observers.add(observer);
       }
       public void delete(Observer observer){
           System.out.println("移除观察者");
           observers.remove(observer);
       }
       public void notify(String news){
           for(Observer observer : observers){
               observer.update(news);
           }
       }
   }
``` 

* **具体主题(ConcreteSubject)**： 当具体主题发生改变时，给集合内的观察者发送通知
```  
public class ConcreteSubject extends Subject{

    public void change(String news){
        System.out.println("主题修改为："+news);
        this.notify(news);
    }
}
``` 

* **抽象观察者(Observer)**：为所有的具体观察者定义更新接口，当得到主题时更新自己
```  
public interface Observer {
    void update(String news);
}
``` 

* **具体观察者(ConcreteObserver)**：实现观察者更新接口
```  
public class ConcreteObserver implements Observer {
    @Override
    public void update(String news) {
        System.out.println("更新主题信息："+ news);
    }
}
``` 

*  测试
``` 
public class Test {
    public static void main(String[] args){
        //主题对象
        ConcreteSubject subject = new ConcreteSubject();
        //观察者对象
        ConcreteObserver observer = new ConcreteObserver();
        subject.add(observer);
        subject.change("今天放假！");
    }
}
``` 

* 运行结果
![subject-observer-final.png](/img/java/subject-observer-final.png)

---

## 推模型与拉模型　　
　　在观察者模式中，又分为推模型和拉模型两种方式：

* 推模型：主题对象向观察者推送主题的详细信息，不管观察者是否需要，推送的信息通常是主题对象的全部或部分数据。上面的代码其实就是推模型。

* 拉模型：主题对象在通知观察者的时候，只传递少量信息。如果观察者需要更具体的信息，由观察者主动到主题对象中获取，
相当于是观察者从主题对象中拉数据。一般这种模型的实现中，会把主题对象自身通过update()方法传递给观察者，这样在观察者需要获取数据的时候，就可以通过这个引用来获取了。

---

## Java 库中的观察者模式
　　在Java的java.util 里提供了Observer接口与Observable类
* Observer ：这个接口只定义了一个方法，即 ``` void update(Observable o, Object arg);``` 方法，当被观察者对象的状态发生变化时，被观察者对象的notifyObservers()方法就会调用这一方法。

* Observable： 被观察者类都是java.util.Observable类的子类。java.util.Observable提供公开的方法支持观察者对象，
这些方法中有两个对Observable的子类非常重要：一个是setChanged()，另一个是notifyObservers()。
第一方法setChanged()被调用之后会设置一个内部标记变量，代表被观察者对象的状态发生了变化。
第二个是notifyObservers()，这个方法被调用时，会调用所有登记过的观察者对象的update()方法，
使这些观察者对象可以更新自己。


