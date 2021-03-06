---
layout:     post
title: "十大经典排序算法Java版总结"
date: 2020-05-16
author: "孙志超"
catalog: false
header-style: text
tags:
  - 算法
---

我想大家学习算法之旅的开端就是各种排序算法吧，的确，排序算法广泛的应用性以及它的简洁基础等性质是初学者的不二之选，那今天我就带着你复习回顾以下各种经典的排序算法吧！

<!--more-->

> 我们的约定：本文所有排序算法操作对象为整数数组，顺序为从小到大


## 1.冒泡排序

**冒泡排序**，顾名思义，就是数据像一个个气泡似的不断地往上冒。大致思路是 : 我们对给定的一个数组，进行n轮冒泡操作，每次操作分别比较相邻两项，如果前一项大于后一项，就将它们交换位置，你可以想象一下这个情景，经过n次比较（如果有必要就进行交换），最终的结果肯定是**最大的那一项**被移动到数组的最右端；那我们重复这个过程，经过n次冒泡操作，就将全部的数据进行了排序，这就是冒泡排序的思路。


<!--more-->


但是我们可以进行一些优化，比如我定义一个名为`flag`的变量监测一轮冒泡操作中交换元素的次数，如果某次冒泡操作中**元素交换**的次数为`0`，那也就意味着所有元素已经完成了排序，我们就可以终止排序结束程序了。

### 具体的代码如下：

```java
 //冒泡排序，a为数组，n为数组的长度
 public static void bobbleSort(int a[],int n){
 if(n<=1) return;
 for(int i=0;i<n;++i){
 //提前退出冒泡排序的标志
 boolean flag=false;
 for(int j=1;j<n-i;++j){
 if(a[j]<a[j-1]){
 int tmp=a[j];a[j]=a[j-1];a[j-1]=tmp;//交换元素
 flag=true;
 }
 }
 if(flag==false) break;//如果没有元素交换，则已经有序，结束
 }
 }
```

### 冒泡排序的特性

- 最好时间复杂度为 O(n) ,这时对应着数组已经是有序的了，只需要一次冒泡操作；最坏时间复杂度为 O(n^2) ,进行 n 次冒泡操作才能完成最终的排序 ；空间复杂度为 O(1) ，是一个原地排序算法

- 冒泡排序是一个稳定的排序算法，因为我们可以控制**如果两个元素相等，就不进行交换**，这就保证了它的稳定性  （这里的稳定性是指，排序算法不改变相等元素在数组中的先后位置，也就是`a==b`，在数组中a的位置比b的位置靠前，那么排序后a的位置仍然靠前，也就是排序不改变它们的相对位置）

- 冒泡排序所需要的**交换操作次数**和数组中的**逆序度**相等

  

## 2.选择排序

选择排序的思想是非常简单的，过程如下：

- 首先通过扫描数组找到其中最小的元素，然后将它与第一个元素交换（如果第一个元素就是最小元素的话那么它就和自己交换）
- 在剩下的数组中找到最小的元素，然后将它与第二个元素交换
- 如此反复n-1次，就达到了排序的效果

### 具体的代码如下：

```java
 public static void selectSort(int[] a,int n){
     for(int i=0;i<n;++i){
         int min=i;//min是最小元素的下标
         //寻找i--n之间最小的元素
         for(int j=i;j<n;++j){
             if(a[j]<a[min]) min=j;
        }
         //交换位置
        { int tmp=a[min];a[min]=a[i];a[i]=tmp; }
    }
 }
```

### 选择排序的特点

- 最好、最坏和平均时间复杂度都是 O(n^2) ; 空间复杂度是 O(1) , 它是一个原地排序算法
- 选择排序不是一个稳定的排序算法，因为每次的交换会改变相等元素之间的相对位置
- 运行时间与输入无关，因为每次扫描并不能为下一次扫描提供什么有用的信息
- 数据的移动是最少的，只有 n 次，交换次数等于数组的长度，这种线性性质是很多排序方法做不到的

## 3.插入排序

现在请回想一下你打扑克牌的情景，每次从桌子上取一张牌，插入到手中合适的位置，经过这种反复的操作，最终你手中的牌恰好是有序的，这正是插入排序的思路。想象着将数组分成**前面的已排序区间**和**后面的未排序区间**，依次遍历这n个元素，每次将一个元素插入到已排序区间**合适**的位置，如此反复，最终将数组排序。

在计算机中，情况会有些复杂，大家清楚，在数组中插入一个元素需要**搬移数据**，这正是插入排序的核心  :  `确定插入点的位置并进行数据搬移`。针对这一点，我们有两种实现思路：

- 通过很多次相邻元素的交换达到查找位置并插入的目的
- 先确定位置，然后数据搬移，最后交换

### 具体的代码如下：

```java
 //第一种做法，利用交换简化排序过程
 public static void insertionSort1(int[] a,int n){
 	for(int i=0;i<n;++i){
 	for(int j=i;j>0&&a[j]<a[j-1];--j)
 	{ 	int tmp=a[j];a[j]=a[j-1];a[j-1]=tmp; }//交换元素
 	}
 }
 
 //第二种做法，数据搬移，然后插入
 public static void insertionSort2(int [] a,int n){
     for(int i=0;i<n;++i){
         int c=a[i],j;
         for(j=i;j>=0&&a[j]>=c;--j) ;//确定c应该插入的位置
         for(int k=i;k>=j+2;--k)//搬移数据
             a[k]=a[k-1];
         a[j+1]=c;//插入数据a[i]
    }
 }
```

### 插入排序的特点

- 时间复杂度：最好情况时间复杂度为 O(n) ，此时对应着数据已经是有序的了，只需要**从头到尾遍历已经有序的数据**；最坏情况时间复杂度为 O(n^2) ,此时对应着数组是倒序的，每次插入都相当于在数组的第一个位置插入新的数据；平均时间复杂度：对于插入排序来说，**每次插入操作都相当于在数组中插入一个数据**，由于**在数组中插入一个数据的平均时间复杂度是 O(n)**,所以执行n次插入操作，平均时间复杂度是 O(n^2).
- 空间复杂度是 O(1) ，插入排序是一个原地排序算法
- 插入排序是一个稳定的排序算法。由于对于值相同的元素，我们可以选择将后面出现的元素，插入到前面出现元素的后面，从而保持原有的前后顺序不变
- 插入排序所需要的时间取决于输入中元素的初始顺序，如果初始序列比较有序的话，那么所需要**交换**或者**搬移数据**的量就会减少，所需时间就会缩短，反正变长
- 插入排序所需要的**交换操作次数**和数组中的**逆序度**相等

## 4.希尔排序

我们可以将希尔排序理解成`魔改版`的插入排序，两位虽然叫着不同的名字（希尔感觉怎么也和插入连不上关系吧！），但是，它们确确实实有着千丝万缕的关系。我们慢慢道来：

不知道你有没有注意到插入排序的一个致命的缺点：**每次只能交换相邻的元素！**，你想象一个情景：如果数组中最小的元素在数组最右端，那岂不是我要进行**n次交换**了！，这可不能忍受，尤其是对于较大的数组这种类似的事情还不少见。于是，人们在插入排序的基础上进行了改进，使它**可以将距离很远的两个元素交换位置**，就这一个特点，很大的提高了效率，人们给它起了新名字`希尔排序`。

我想，你听过了我上面的叙述后，大概对希尔排序不再陌生了吧。而在它的实现过程中，我们考虑的是`h有序数组`,即相隔 h 的子数组是有序的。我们通过控制 h 的值，使它逐渐减小，也就是**有序的间距逐渐变小**，最后以`h=1`（即插入排序）结尾，以求**保证最终结果的有序性**。具体的代码见下文：

### 具体的代码如下：

```java
 //希尔排序
 public static void shellSort(int [] a,int n){
     int h=1;
     while(h<=n/3) h=3*h+1;//初始化间隔h，目的是为了下文的统一
     while(h>=1){
     for(int i=0;i<n;++i){
         for(int j=i;j>=h&&a[j]<a[j-h];j=j-h){
             int tmp=a[j];a[j]=a[j-h];a[j-h]=tmp;//交换元素
        }
    }
     h=h/3;//减小间距
    }
 }
```

### 希尔排序的特点：

- 希尔排序的时间复杂度与我们选择的`h`有关，例如我们之前的代码中`h`取值为 3 ，但是有一点可以确定 : `希尔排序的时间复杂度达不到平方级别`,这是一个比较有用的结论，它告诉了我们希尔排序的优势；我们`h==3`的情况下，最坏情况时间复杂度为`O(n^(3/2))`，可见，一个对于插入排序小小的改变就突破了平方级的运行时间

- 希尔排序是一个原地排序算法，空间复杂度为 O(1)

- 希尔排序**不是**一个稳定的排序算法！！！！！它与插入排序不同

- 希尔排序对于较大的数组性能比较好。这也没有什么值得惊讶的，还记得我们最开始的烦恼吗？**插入排序每次只能交换相邻元素**，希尔排序就很好地解决了这个问题

  

## 5.归并排序

所谓归并简单来说就是**保持有序的合并** 。假如我们要将数组 a 排序，可以考虑如下步骤:

- 将数组 a 分为数组 b 和数组 c
- 分别将数组 b 和数组 c 排序
- 将两个有序的子数组 b 和 c **归并**成一个有序的大数组

如此一来，数组 a 就自然变得有序了。利用递归的思想，我们不难想到以下排序的思路：

```java
 //归并排序
 private static void sort(int [] a,int lo,int hi){
     if(lo>=hi) return;
     int mid=(lo+hi)/2;
     sort(a,lo,mid);//将左半部分排序
     sort(a,mid+1,hi);//将右半部分排序
     merge(a,lo,mid,hi);//归并结果
 }
```



上面这个代码巧妙地利用递归思想实现了对数组 a 排序。我们下面只需要专注于归并算法如何实现，也就是**已知两个数组 b 和 c 均从小到大有序排列，将它们合并成一个有序的数组 a**，我们这里利用一个`辅助数组 aux[]`实现了`原地归并算法`,具体的代码如下：

```java
 //原地归并算法,任务是将数组 a[lo---mid] 与数组 a[mid+1---hi]合并
 private static void merge(int [] a,int lo,int mid,int hi){
     //将数组 a 中的元素复制到数组 aux 中
     for(int i=lo;i<=hi;++i)
         aux[i]=a[i];
     //归并
     int i=lo,j=mid+1;
     for(int k=lo;k<=hi;++k){
         if(i>mid) a[k]=aux[j++];
         else if(j>hi) a[k]=aux[i++];
         else if(aux[j]<aux[i]) a[k]=aux[j++];
         else a[k]=aux[i++];
    }
 }
```

最后，我们的归并排序的代码如下：

```java
 //归并排序的最终代码
 private static int [] aux;
 public static void mergeSort(int [] a,int n){
     aux=new int[n];//申请一个数组提供给 merge算法
     sort(a,0,n-1);
 }
```

梳理一下原地归并排序的思路：通过辅助数组实现`merge函数`；`sort函数`内嵌`merge函数`，通过不断递归地调用`sort函数`实现排序。

上文我们实现的归并排序被称为**自顶向下的归并排序**，我们先对大数组进行分割，再对小数组进行排序。其实归并排序还有另一种版本**自底向上的归并排序**，我们从最小的数组做起，一个一个归并，两个两个归并，最后是全部元素归并。其具体的代码如下：

```java
 public static void mergeSort2(int [] a,int n){
     //进行logn次两两归并
     aux=new int[n];
     //size是子数组的长度，lo是子数组的索引（即第一个元素）
     for(int size=1;size<n;size=size*2){
         for(int lo=0;li<n-size;lo+=size+size){
             merge(a,lo,lo+size-1,Math.min(lo+size+size-1,n-1));
        }
    }
 }
```

它的思路就是从底层归并，每次遍历完整个数组之后，size就翻倍，最后一步归并完就是整个数组了

### 归并排序的特点

- 我们先来分析以下它的时间复杂度。我们假设对 n 个元素进行归并排序所需要的时间是 T(n)，那分解成两个子数组排序的时间都是 T(n/2)。我们可以发现，merge() 函数合并两个有序子数组的时间复杂度是 O(n)。所以，归并排序的时间复杂度的计算公式就是：

  ```
   T(1) = C；  n=1时，只需要常量级的执行时间，所以表示为C。
   T(n) = 2*T(n/2) + n；n>1
  ```

  然后，我们通过数学的推导可以得到结论 : **归并排序的时间复杂度为 O(nlogn)** , 而且：归并排序的执行效率与要排序的原始数组的有序程度无关，所以其时间复杂度是**非常稳定**的，不管是最好情况、最坏情况，还是平均情况，时间复杂度都是 O(nlogn)。g

- 由于我们使用了原地归并的算法，所以空间复杂度为 O(1) ,是一个原地排序算法，当然，不同的归并算法导致的结果也不同。

- 归并排序是一个稳定的排序算法 ，因为在归并的过程中，我们可以认为地控制**当两个元素相等时，先放置前面地元素**。

## 6.快速排序

快速排序是一种应用非常广泛的排序算法，我们接下来分析一下它的思路：

- 找到一个基准元素 **K** ，将原数组 a 分成两个数组 b 和 c ，使得b中的元素都小于 **K** , c中的元素都大于 **K**，即 `a[0---n-1] ---> b+K+c`
- 将数组 b 和数组 c 分别排序

具体的代码如下：

```java
 //快速排序的实现
 public static void quickSort(int [] a,int n){
     sort(a,0,n-1);
 }
 //排序方法
 private static void sort(int [] a,int lo,int hi){
     if(lo>=hi) return;
     int i=partition(a,lo,hi);//切分
     sort(a,lo,i);//将左半部分排序
     sort(a,i+1,hi);//将右半部分排序
 }
```



快速排序主要的问题在于**切分方法的实现**。我们的做法是：

- 取定待切分数组的最左端元素为切分元素

- 通过左右扫描指针的做法实现切分

  具体的代码如下：

- - 我们从数组的左端开始向右扫描直到找到一个大于等 于它的元素，再从数组的右端开始向左扫描直到找到一个 小于等于它的元素。然后我们交换它们的位置
  - 如此继续，我们就可以保证左指针`i` 的左侧元素都不大于切分元素，右指针 `j` 的右侧元素都 不小于切分元素
  - 当两个指针相遇时，我们只需要将切分 元素 `a[lo]` 和左子数组最右侧的元素 `a[j]` 交换然后返回切分位置 `j` 即可

```java
 private static int partition(int [] a,int lo,int hi){
     int c=a[lo];//切分基准
     int i=lo,j=hi+1;//左右扫描指针
     while(true){
         while(a[--j]>=c) if(j==lo) break;
         while(a[++i]<=c) if(i==hi) break;
         if(i>=j) break;
        { int tmp=a[j];a[j]=a[i];a[i]=tmp; }//交换元素
    }
     a[lo]=a[j];a[j]=c;//将c放入合适的位置
     retrun j;//返回切分元素的位置
     
 }
```

### 快速排序的特点

- 快速排序的时间复杂度比较依赖切分点的位置，你可以想一下，如果给定待排序的数组是从大到小完全逆序的，比如4，3，2，1。我们需要进行大约 n 次分区操作，才能完成快排的整个过程。每次分区我们平均要扫描大约 n/2 个元素，这种情况下，快排的时间复杂度就是 O(n^2)。

- 一般情况下，如果切分点的位置比较靠近中间，快排和归并排序其实是一样的

  ```
   T(1) = C；   n=1时，只需要常量级的执行时间，所以表示为C。
   T(n) = 2*T(n/2) + n； n>1
  ```

  由此得到其时间复杂度为 O(n^2)。当然，T(n) 在大部分情况下的时间复杂度都可以做到 O(nlogn)，只有在极端情况下，才会退化到 O(n2)。我们也可以事先随机化数组来避免极端情况的发生

- 快速排序的空间复杂度为 O(1)，是一个原地排序算法

- 快速排序不是一个稳定的排序算法



## 7.堆排序

由于堆排序是基于数据结构**堆**的一个排序，我打算在介绍堆的时候再详细地讲解，这里就暂且略过了

## 8.桶排序

举个栗子，我们的任务是将 40 个 100 以内的数字排序。我们可以这么做：

- 将0~~20以内的数字放入第一个桶内
- 将21~~40以内的数字放入第二个桶内
- 依次反复直到将这40个数字放入 5 个桶内
- 利用某种排序方法（本文用的快速排序）将桶内元素排序
- 从桶内按照顺序将数字取出依次放入一个原数组

经过以上操作，原数组是不是已经变得有序了呢？

我们可以将桶排序与快速排序对比，快速排序是将集合拆分为两个值域，这里称为两个桶，再分别对两个桶进行排序，最终完成排序。桶排序则是将集合拆分为多个桶，对每个桶进行排序，则完成排序过程。两者不同之处在于，快排是在集合本身上进行排序，属于原地排序方式，且对每个桶的排序方式也是快排。桶排序则是提供了额外的操作空间，在额外空间上对桶进行排序，避免了构成桶过程的元素比较和交换操作，同时可以自主选择恰当的排序算法对桶进行排序。

### 具体代码如下：

- 桶的构造
- 数据的分配
- 桶内元素排序
- 将桶内数据拿出来

```java
 public class BucketSort {
     /**
      * 桶排序
      *
      * @param arr 数组
      * @param bucketSize 桶容量
      */
     public static void bucketSort(int[] arr, int bucketSize) {
         if (arr.length <= 1) {
             return;
        }
 
         // 数组最小值
         int minValue = arr[0];
         // 数组最大值
         int maxValue = arr[1];
         for (int i = 0; i < arr.length; i++) {
             if (arr[i] < minValue) {
                 minValue = arr[i];
            } else if (arr[i] > maxValue) {
                 maxValue = arr[i];
            }
        }
 
         // 桶数量
         int bucketCount = (maxValue - minValue) / bucketSize + 1;
         //桶
         int[][] buckets = new int[bucketCount][bucketSize];
         //每个桶中元素的个数
         int[] indexArr = new int[bucketCount];
 
         // 将数组中值分配到各个桶里
         for (int i = 0; i < arr.length; i++) {
             //计算出这个元素arr[i]应该处于哪个桶中
             int bucketIndex = (arr[i] - minValue) / bucketSize;
             buckets[bucketIndex][indexArr[bucketIndex]++] = arr[i];
        }
 
         // 对每个桶进行排序，这里使用了快速排序
         int k = 0;
         for (int i = 0; i < buckets.length; i++) {
             if (indexArr[i] == 0) {
                 continue;
            }
             //在这个桶排序中，我们用到了快速排序去完成桶内的排序
             Sort.quickSort(buckets[i], indexArr[i] - 1);
             for (int j = 0; j < indexArr[i]; j++) {
                 arr[k++] = buckets[i][j];
            }
        }
    }
 }
```

### 桶排序的特点

桶排序对要排序数据的要求是非常苛刻的，它要求桶与桶之间有着天然的大小关系，只有这样，在每个桶内的数据都排序完之后，桶与桶之间的数据不需要再进行排序。另外，数据在桶内的均匀分布的程度也是影响桶排序的一个重要因素，如果分布极端不均匀，桶排序就会由  `n` 退化为 `nlog(n)` 。

- 最佳情况：T(n) = O(n)。当输入的数据可以均匀的分配到每一个桶中。

- 最差情况：T(n) = O(nlogn)。当输入的数据被分配到了同一个桶中。
- 平均情况：T(n) = O(n)。

## 9.计数排序

计数排序可以看出桶排序的一个特例，只不过在这里桶的大小变成了 1 。假如我们的任务是将40个20以内的数字排序，我们可以申请一个数组 `book[21]`,然后通过遍历数组用 `book[i]` 的值存储数字 `i` 出现的次数，最后将数据依次放入原数组。以下是两种不同的版本，第一种比较容易理解，但是不稳定，第二种是稳定的排序算法，但是理解起来不是很容易。

### 不稳定的计数排序：

- 查找数据范围并申请数组
- 遍历数组所有元素计算 `book[i]`
- 将结果拷贝回 a 数组

```java
 public class CountingSort {
 
   // 计数排序，a是数组，n是数组大小。假设数组中存储的都是非负整数。
   public static void countingSort(int[] a, int n) {
     if (n <= 1) return;
 
     // 查找数组中数据的范围
     int max = a[0];
     for (int i = 1; i < n; ++i) {
       if (max < a[i]) {
         max = a[i];
      }
    }
 
     // 申请一个计数数组book，下标大小[0,max]
     int[] book = new int[max + 1];
 
     // 计算每个元素的个数，放入book中
     for (int i = 0; i < n; ++i) {
       book[a[i]]++;
    }
 
     // 临时数组r，存储排序之后的结果
     int[] r = new int[n];
 
     //将结果储存在数组 r 中
       int index=0;
       for(int i=0;i<n;++i){
           for(int j=0;j<book[i];++j){
               r[index++]=a[j];
          }
      }
      // 将结果拷贝会a数组
     for (int i = 0; i < n; ++i) {
       a[i] = r[i];
    }
  }
 }
```

### 稳定的计数排序

- 查找数据范围并申请数组
- 遍历数组所有元素计算 `book[i]`
- 依次累加
- 将结果拷贝回临时数组（重点）
- 将结果从临时数组拷贝回 a 数组

```java
 public class CountingSort {
 
   // 计数排序，a是数组，n是数组大小。假设数组中存储的都是非负整数。
   public static void countingSort(int[] a, int n) {
     if (n <= 1) return;
 
     // 查找数组中数据的范围
     int max = a[0];
     for (int i = 1; i < n; ++i) {
       if (max < a[i]) {
         max = a[i];
      }
    }
 
     // 申请一个计数数组book，下标大小[0,max]
     int[] book = new int[max + 1];
 
     // 计算每个元素的个数，放入book中
     for (int i = 0; i < n; ++i) {
       book[a[i]]++;
    }
 
     // 依次累加
     for (int i = 1; i < max + 1; ++i) {
       book[i] = book[i-1] + book[i];
    }
 
     // 临时数组r，存储排序之后的结果
     int[] r = new int[n];
     // 计算排序的关键步骤了，有点难理解
     for (int i = n - 1; i >= 0; --i) {
       int index = book[a[i]]-1;
       r[index] = a[i];
       book[a[i]]--;
    }
 
     // 将结果拷贝会a数组
     for (int i = 0; i < n; ++i) {
       a[i] = r[i];
    }
  }
 }
```

### 计数排序的特点

- 计数排序只能用在数据范围不大的场景中，如果数据范围 k 比要排序的数据 n 大很多，就不适合用计数排序了。
- 计数排序只能给非负整数排序，如果要排序的数据是其他类型的，要将其在不改变相对大小的情况下，转化为非负整数。（比如可以同时加上一个数字）

## 10.基数排序

基数排序是一种非比较型整数排序算法，其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。

> 通俗点讲，就是让你去比较几个多位数的大小关系，你会怎么做？肯定是先看最高位喽，如果 a 的最高位最大，那最终结果 a 肯定最大。所谓的基数排序就是如此，不过一些细节不一样而已。

### 使用条件

- 要求数据可以分割独立的`位`来比较；
- 位之间由递进关系，如果数据 a 的高位比数据 b 大，那么剩下的位置就不用比较了；
- 每一位的数据范围不能太大，要可以用线性排序，否则基数排序的时间复杂度无法做到 O(n)。

### 方案

按照优先从高位或低位来排序有两种实现方案:

- MSD：由高位为基底，先按 k1 排序分组，同一组中记录, 关键码 k1 相等，再对各组按 k2 排序分成子组, 之后，对后面的关键码继续这样的排序分组，直到按最次位关键码 kd 对各子组排序后，再将各组连接起来，便得到一个有序序列。MSD 方式适用于位数多的序列。
- LSD：由低位为基底，先从 kd 开始排序，再对 kd - 1 进行排序，依次重复，直到对 k1 排序后便得到一个有序序列。LSD 方式适用于位数少的序列。

### 具体的代码实现：

- 找到数组中的最大值
- 从最低位开始，对每一位都进行计数排序

```java
 public class RadixSort {
 
     /**
      * 基数排序
      *
      * @param arr
      */
     public static void radixSort(int[] arr) {
         int max = arr[0];
         for (int i = 0; i < arr.length; i++) {
             if (arr[i] > max) {
                 max = arr[i];
            }
        }
 
         // 从个位开始，对数组arr按"指数"进行排序
         for (int exp = 1; max / exp > 0; exp *= 10) {
             countingSort(arr, exp);
        }
    }
 
     /**
      * 计数排序-对数组按照"某个位数"进行排序
      *
      * @param arr
      * @param exp 指数
      */
     public static void countingSort(int[] arr, int exp) {
         if (arr.length <= 1) {
             return;
        }
 
         // 计算每个元素的个数
         int[] book = new int[10];
         for (int i = 0; i < arr.length; i++) {
             book[(arr[i] / exp) % 10]++;
        }
 
         // 计算排序后的位置
         for (int i = 1; i < book.length; i++) {
             book[i] += book[i - 1];
        }
 
         // 临时数组r，存储排序之后的结果
         int[] r = new int[arr.length];
         for (int i = arr.length - 1; i >= 0; i--) {
             r[book[(arr[i] / exp) % 10] - 1] = arr[i];
             book[(arr[i] / exp) % 10]--;
        }
 
         for (int i = 0; i < arr.length; i++) {
             arr[i] = r[i];
        }
    }
 }
```

### 基数排序的特点

- 基数排序的空间复杂度为 O(n + k)，所以不是原地排序算法。

- 基数排序不改变相同元素之间的相对顺序，因此它是稳定的排序算法。

- 根据每一位来排序，我们可以用刚讲过的桶排序或者计数排序，它们的时间复杂度可以做到 O(n)。如果要排序的数据有 k 位，那我们就需要 k 次桶排序或者计数排序，总的时间复杂度是 O(k*n)。当 k 不大的时候，比如要排序我们的手机号码，k 最大就是 11，所以基数排序的时间复杂度就近似于 O(n)。

  最佳情况：T(n) = O(n * k) 。

  最差情况：T(n) = O(n * k) 。

  平均情况：T(n) = O(n * k) 。

  其中，k 是待排序列最大值。

全文完