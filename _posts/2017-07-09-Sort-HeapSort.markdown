---
layout:     post
title:      "堆排序"
subtitle:   ""
date:       2017-07-09
author:     "Will"
header-img: "img/java/java.png"
catalog: true
tags:
    - Sort
    
---

> “walk beside you ”


## 前言

　　你现在混日子，小心将来日子混了你。
                                ------  老马

---

## 正文
   堆排序（Heap Sort）是指利用堆积树这种数据结构所设计的一种排序算法，它是一种选择排序。
堆分为大根堆、小根堆，为完全二叉树。
   * 大根堆：每个节点的值都小于或等于其父节点的值 （升序排序使用）
   * 小根堆：每个节点的值都大于或等于父节点的值（降序排序使用）

#### 原理分析
例如大根堆：
   * 初始化大根堆

        1. 把一个无序的数组 ** [3,1,6,5,9,2,4,8,7] ** 按照下标顺序构建为一颗完全二叉树![二叉数](/img/heapsort/heapsort_01.png)
        2. 然后从最后一个非叶子结点开始，每次都是从父结点、左孩子、右孩子中进行比较交换，交换可能会引起孩子结点不满足堆的性质，所以每次交换之后需要重新对被交换的孩子结点进行调整。
        3. 非叶子节点的选取为（n - 1）/2  向下取整，n 为最后元素的下标：因为 n = 8 ，所以第一次非叶子节点为 3 ![二叉数](/img/heapsort/heapsort_02.png)
        4. 父节点分别于它的左右子节点进行对比，把最大的调整到父节点位置 ![二叉数](/img/heapsort/heapsort_03.png)
        5. 第二次选取的非叶子节点为 2 ，由于下标为 2 的非叶子节点各个子节点均小于非叶子节点，所以无需调整
        6. 依次类推最终生成的二叉树需满足 ** 每个节点的值都小于或等于其父节点的值  ** ![二叉数](/img/heapsort/heapsort_04.png)
   * 进行堆排序

        1. 上面生成的大根堆为无序的堆，我们需要对其进行排序，以达到我们的要求
        2. 首先交换堆顶元素与最后一个元素，得到  ![二叉数](/img/heapsort/heapsort_05.png)
        3. 这样没有颜色的区域形成了一个新的二叉树，我们再次把它（没有颜色的区域）调整为大根堆 ![二叉数](/img/heapsort/heapsort_06.png)
        4. 重复上面的动作直到最后无颜色的区域元素剩下一个，最后我们会等到一个排好顺序的堆  ![二叉数](/img/heapsort/heapsort_07.png)

#### 时间复杂度
   * 建堆，建堆是不断调整堆的过程，从len/2处开始调整，一直到第一个节点，此处len是堆中元素的个数。建堆的过程是线性的过程，从len/2到0处一直调用调整堆的过程，相当于o(h1)+o(h2)…+o(hlen/2) 其中h表示节点的深度，len/2表示节点的个数，这是一个求和的过程，结果是线性的O(n)。
   * 调整堆：调整堆在构建堆的过程中会用到，而且在堆排序过程中也会用到。利用的思想是比较节点i和它的孩子节点left(i),right(i)，选出三者最大(或者最小)者，如果最大（小）值不是节点i而是它的一个孩子节点，那边交互节点i和该节点，然后再调用调整堆过程，这是一个递归的过程。调整堆的过程时间复杂度与堆的深度有关系，是lgn的操作，因为是沿着深度方向进行调整的。
   * 堆排序：堆排序是利用上面的两个过程来进行的。首先是根据元素构建堆。然后将堆的根节点取出(一般是与最后一个节点进行交换)，将前面len-1个节点继续进行堆调整的过程，然后再将根节点取出，这样一直到所有节点都取出。堆排序过程的时间复杂度是O(nlgn)。因为建堆的时间复杂度是O(n)（调用一次）；调整堆的时间复杂度是lgn，调用了n-1次，所以堆排序的时间复杂度是O(nlgn)

#### 代码

 ```
        public class HeapSort {

            /**
             * 构建大根堆
             * @param array  数组
             * @param parent 父节点
             * @param length 数组指定长度
             */
            public static void buildBigHeap(int[] array,int parent,int length){

                int left = (parent << 1) + 1;//左节点下标
                int right = (parent << 1) +2;//右节点下标

                int tmp = parent;
                if(left < length && array[parent] < array[left]){
                    tmp = left;
                }
                if(right < length && array[tmp] < array[right]){
                    tmp = right;
                }
                // 交换位置
                if (tmp == parent){
                    return;
                }
                int temp=array[parent];
                array[parent] = array[tmp];
                array[tmp] = temp;
                // 如果被调整的节点为 非叶子节点则需要 对其重新调整
                buildBigHeap(array,tmp,length);
            }

          public static void heapSort(int[] array){
              /*
                堆顶与最后一个元素交换位置
                然后对其他区域重新生成大根堆
               */
              for(int i = array.length - 1; i>0; i--){
                  // 交换位置
                  int temp = array[0];
                  array[0] = array[i];
                  array[i] = temp;
                  // 重新生成大根堆
                  buildBigHeap(array,0,i);
              }

          }

          public static void main(String[] arg){
              int[] array = {3,1,6,5,9,2,4,8,7};
              int startParent =  array.length/2-1;
              for(int i = startParent;i>=0;i--){
                  buildBigHeap(array,i,array.length);
              }
              heapSort(array);
              for (int i : array){
                  System.out.print(i+"  ");
              }
          }

        }

  ```


