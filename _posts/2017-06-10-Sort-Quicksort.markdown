---
layout:     post
title:      "快速排序"
subtitle:   ""
date:       2017-06-10
author:     "Will"
header-img: "img/java/java.png"
catalog: true
tags:
    - Sort
    
---

> “walk beside you ”


## 前言
   空山新雨后，天气晚来秋。
   明月松间照，清泉石上流。
   竹喧归浣女，莲动下渔舟。
   随意春芳歇，王孙自可留。
    王维 《山居秋暝》

---

## 正文
快速排序（Quick Sort），是对冒泡排序的一种改进。
快速排序是一种非常高效的排序算法。采用“分而治之”的思想，把数组不断的拆分做处理。
#### 原理分析

1. 假如数组 N ，我们需要选定一个数为关键数，通常为第一个数，或者最后一个数
2. 扫描数组，将数组 N 分成两部分：一部分比 关键数小，一部分比关健数大或者等于
3. 在对这两部分数组重复执行上述动作，直到数据有序

 ```
    原数组 ： [4,1,6,5,2,8,3,9,7]  ,取 关键数为：4
 第一次：key = 4 ,i = 0, j =8
    [4   1   6   5   2   8   3   9   7   ](key 与 7 比较，不交换位置) key = 4 ,i = 0, j = 7
    [4   1   6   5   2   8   3   9   7   ](key 与 9 比较，不交换位置) key = 4 ,i = 0, j = 6
    [3   1   6   5   2   8   4   9   7   ](key 与 3 比较，交换位置)   key = 4 ,i = 1, j = 6
    [3   1   6   5   2   8   4   9   7   ](key 与 1 比较，不交换位置) key = 4 ,i = 2, j = 6
    [3   1   4   5   2   8   6   9   7   ](key 与 6 比较，交换位置)   key = 4 ,i = 2, j = 5
    [3   1   4   5   2   8   6   9   7   ](key 与 8 比较，不交换位置) key = 4 ,i = 2, j = 4
    [3   1   2   5   4   8   6   9   7   ](key 与 2 比较，交换位置)   key = 4 ,i = 3, j = 4
    [3   1   2   4   5   8   6   9   7   ](key 与 5 比较，交换位置)   key = 4 ,i = 3, j = 3
 第二次：key = 3 ,i = 0, j =2
   [2   1   3   4   5   8   6   9   7   ]
   [2   1   3   4   5   8   6   9   7   ]
 第三次：key = 2 ,i = 0, j =1
   [1   2   3   4   5   8   6   9   7   ]
 第三次：key = 5 ,i = 4, j =8
   [1   2   3   4   5   8   6   9   7   ]
   [1   2   3   4   5   8   6   9   7   ]
   [1   2   3   4   5   8   6   9   7   ]
   [1   2   3   4   5   8   6   9   7   ]
 第三次：key = 8 ,i = 5, j =8
   [1   2   3   4   5   7   6   9   8   ]
   [1   2   3   4   5   7   6   9   8   ]
   [1   2   3   4   5   7   6   8   9   ]
 第三次：key = 7 ,i = 5, j =6
   [1   2   3   4   5   6   7   8   9   ]


 ```
#### 时间复杂度

规定整个过程比较次数为 C

* 若选取的关键数正好为数组的中间值，所以需要比较的次数 C= nlogn ，所以时间复杂度为 O（nlogn）
* 若每次分成的两个数组其中的一个长度总为1，则需要比较的此时  C = (n-1) + (n-2) + ...+ 2 + 1  = (n-1)*n/2  所以时间复杂度为 O(n^2)

#### 代码

 ```
        public static void  quick(int[] array,int low ,int high){
                if(low >= high ){
                    return;
                }
                int h = high;
                int l = low;
                /*
                    当前位置
                 */
                int keyIndex = low;
                int keyValue = array[keyIndex];

                while (low < high){
                /*
                    如果当前位置 为 low 则 与 高位 比较
                    如果为 high 则与 低位比较
                 */
                    if(keyIndex == low){
                        if(keyValue > array[high]){
                            array[keyIndex] = array[high];
                            array[high] = keyValue;
                            keyIndex = high;
                            low ++;
                        }else{
                            high --;
                        }
                    }
                    if(keyIndex == high){
                        if(keyValue < array[low]){
                            array[keyIndex] = array[low];
                            array[low] = keyValue;
                            keyIndex = low;
                            high --;
                        }else{
                            low ++;
                        }
                    }
                }
               /*
               小于关键数的数组
                */
               quick(array,l,keyIndex-1);
               /*
               大于关键数的数组
                */
               quick(array,keyIndex+1,h);
            }

  ```


