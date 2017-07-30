---
layout:     post
title:      "Java数据类型"
subtitle:   ""
date:       2017-05-20
author:     "Will"
header-img: "img/java/java.png"
catalog: true
tags:
    - Java
---

> “walk beside you ”


# 前言
    天天上班写代码、数据类型更是天天接触使用。现在整理下补全自己的不足。
---

# 正文
Java有两大数据类型：
* 内置数据类型 ：
    * 如果是局部变量则其变量名及值是放在方法栈中
    * 如果是全局变量则其变量名及其值放在堆内存中
* 引用数据类型 ：
    * 如果是局部变量则所声明的变量（该变量实际上是在方法中存储的是内存地址值）是放在方法的栈中，该变量所指向的对象是放在堆类存中、
    * 如果是全局变量其声明的变量仍然会存储一个内存地址值，该内存地址值指向所引用的对象。引用变量名和对应的对象仍然存储在相应的堆中　　

## 内置数据类型
Java提供了八种基本数据类型：
* 数字类型（byte、short、int、long、float、double）
* 字符类型（char）
* 布尔类型（boolean）

运算符对基本类型的影响。当使用+、-、*、/、%运算符对基本类型进行运算时，遵循如下规则：
* A：两个操作数中有一个是double类型的，另一个将会被转换成double类型，并且结果也是double类型
* B：两个操作数中有一个是float类型的，另一个将会被转换成float类型，并且结果也是float类型
* C：两个操作数中有一个是long类型的，另一个将会被转换成long类型，并且结果也是long类型
* D：两个操作数（包括byte、short、int、char）都将会被转换成int类型，并且结果也是int类型

### byte
* byte 占据1个字节、8位（bits）、有符号、以二进制补码表示的整数
* 最大值 ``` MAX_VALUE = 127; ``` （2^7-1）、最小值 ``` MIN_VALUE = -128;  ``` （-2^7）
* byte 类型用在大型数组中节约空间，主要代替整数，因为 byte 变量占用的空间只有 int 类型的四分之一

#### Byte构造方法
* ``` Byte(byte value)  ``` 构造一个新分配的 Byte 对象，以表示指定的 byte 值
* ``` Byte(String s)   ``` 构造一个新分配的 Byte 对象，以表示 String 参数所指示的 byte 值

#### Byte方法
* ```    byte	byteValue()```
作为一个 byte 返回此 Byte 的值。
* ```    int	compareTo(Byte anotherByte)```
在数字上比较两个 Byte 对象。
* ```   static Byte	decode(String nm)```
将 String 解码为 Byte。
* ```   double	doubleValue()```
作为一个 double 返回此 Byte 的值。
* ```    boolean	equals(Object obj)```
将此对象与指定对象比较。
* ```    float	floatValue()```
作为一个 float 返回此 Byte 的值。
* ```    int	hashCode()```
返回此 Byte 的哈希码。
* ```   int	intValue()```
作为一个 int 返回此 Byte 的值。
* ```   long	longValue()```
作为一个 long 返回此 Byte 的值。
* ```    static byte	parseByte(String s)```
将 string 参数解析为有符号的十进制 byte。
* ```   static byte	parseByte(String s, int radix)```
将 string 参数解析为一个有符号的 byte，其基数由第二个参数指定。
* ```    short	shortValue()```
作为一个 short 返回此 Byte 的值。
* ```    String	toString()```
返回表示此 Byte 的值的 String 对象。
* ```   static String	toString(byte b)```
返回表示指定 byte 的一个新 String 对象。
* ```   static Byte	valueOf(byte b)```
返回表示指定 byte 值的一个 Byte 实例。
* ```   static Byte	valueOf(String s)```
返回一个保持指定 String 所给出的值的 Byte 对象。
* ```   static Byte	valueOf(String s, int radix)```
返回一个 Byte 对象，该对象保持从指定的 String 中提取的值，该值是在用第二个参数所给定的基数对指定字符串进行解析时提取的。

#### Byte运算
 ```
    byte a=1;
    byte b=2;
    byte c;
    c=a+b;  // 错误 java: 不兼容的类型: 从int转换到byte可能会有损失   参照 上面的 D 条
    c=a+1;  // 错误 java: 不兼容的类型: 从int转换到byte可能会有损失  参照 上面的 D 条
    c=3+1; // 3+1是在编译时就可以确定的常量，且范围没有超出byte的取值范围，这个关系式就相当于c=4，因而不会报错
```


### short

### int

### long

### float

### double

### char

### boolean


