---
layout:     post
title:      "Bitmap算法"
subtitle:   ""
date:       2017-08-27
author:     "Will"
header-img: "img/java/java.png"
catalog: true
tags:
    - Sort
    
---

> “walk beside you ”


## 前言

　　人生在世只有一次，不必勉强选择自己不喜欢的路，随性而生或随性而死都没关系，不过，无论选择哪条路，都不要忘记保护自己所珍惜的人。
                                ------  三代火影

---

## 正文

   位图：位图的原理就是用一个bit来标识一个数字是否存在，采用一个bit来存储一个数据，所以这样可以大大的节省空间。
   例如一个int型有32bit，那么就可以用这32个bit来存储0~31这些整型数据，所以可以将1~31这些数据仅用1个bit来存储，这样节省了空间。
   例如要存储1，11，21


 ```
   0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1

   0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0

   0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0

 ```

   优点：1.运算效率高，不许进行比较和移位；2.占用内存少。

   缺点：所有的数据不能重复。即不可对重复的数据进行排序和查找。

#### 使用场景

例如：给一台普通PC，2G内存，要求处理一个包含20亿个不重复并且没有排过序的无符号的int整数，给出一个整数，问如果快速地判断这个整数是否在文件20亿个数据当中？
常规解决方案把20亿个数字放到内存，然后判断。这样的话 20亿个数字 （2000000000*4/1024/1024/1024） = 7.45G  远远大于 2G内存
如果使用Bitmap算法 使用bit来表示 则 （2000000000/8/1024/1024）=238.4M
在Java 中 可以使用BitSet来处理Bitmap算法

#### 代码

 ```
public class BitMap {

    byte[] tem;

    public BitMap(int length) {
        this.tem = new byte[length];
    }

    public void add(int num) {
        if (num < tem.length) {
            if (tem[num] != 1) {
                tem[num] = 1;
            }
        }
    }

    public boolean contain(int num) {
        if (num < tem.length) {
            if (tem[num] == 1) {
                return true;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        /*运行前内存*/
        long beforeMemory = Runtime.getRuntime().totalMemory();
        long start1=System.currentTimeMillis();
        BitSet set = new BitSet(2000000000);
        for (int i = 0; i < 2000000000; i++) {
        /*假设999999这个数不在20亿个数里面*/
            if (i != 999999) {
                set.set(i, true);
            }
        }
        /*创建20亿个数后所占内存*/
        long afterMemory = Runtime.getRuntime().totalMemory();
        long end1=System.currentTimeMillis();
        System.out.println("总共内存使用:" + (afterMemory - beforeMemory) / 1024 / 1024 + "MB");
        System.out.println("存入内存耗时:"+(end1-start1)+"毫秒");
        long start2 = System.currentTimeMillis();
        boolean isExit1=set.get(999999);
        boolean isExit2=set.get(900000);

        long end2 = System.currentTimeMillis();

        System.out.println(isExit1);
        System.out.println("20个亿中"+(isExit1?"包含":"不包含")+999999);
        System.out.println("20个亿中"+(isExit2?"包含":"不包含")+900000);
        System.out.println("查询用时:"+(end2 - start2)+"毫秒");
    }
}

  ```

输出结果：

  ```
    总共内存使用:238MB
    存入内存耗时:8812毫秒
    false
    20个亿中不包含999999
    20个亿中包含900000
    查询用时:0毫秒

  ```

  可见其效率之高