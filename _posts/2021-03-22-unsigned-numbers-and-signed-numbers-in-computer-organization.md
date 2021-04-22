---
layout:     post
title: "计算机组成：无符号数和有符号数"
date: 2021-03-22
author: "孙志超"
catalog: false
header-style: text
tags:
  - 计算机组成原理
---

![image-20210321142937755](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321142937755.png)

[TOC]

## 1.无符号数

- 直接用二进制表示。

- 寄存器的位数反映了无符号数的表示范围。

![image-20210321143140880](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321143140880.png)

## 2.有符号数

### 1.机器数与真值

保存在计算机中的数字我们称为**机器数** ，我们平时用的带有符号的真实的值称为**真值** 。在计算机中用**0或1** 表示**数字的符号** 。

大家注意，在计算机中没有专门的硬件用于表示小数点，**计算机中的小数点都是以约定的方式给出的**。

![image-20210321143632806](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321143632806.png)

### 2.原码表示法

#### 整数

我们这里以数学的方式给出原码表示法的定义

![image-20210321144438773](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321144438773.png)

注意到对于一个位数为 n 的正整数，即使所有数位上的值都是 1 ，这是值达到最大值  $$2^n-1$$ ,所以上面第一个范围是$$ 2^n>x>=0$$ 

根据上面公式可以看出，**如果 $$x$$ 的值为 0 ，则有两种表示方式** 。

下面是一个具体的实例：

![image-20210321150831043](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321150831043.png)

我们不难发现，

1. 对于正整数而言，取符号位为 0 ，后面直接加上真值的二进制表示即可
2. 对于负整数而言，取符号位为 1，后面直接加上真值的绝对值的二进制表示即可

由此，我们得出结论：**原码是带符号的绝对值表示！** 

#### 小数

接下来我们来看一下小数的原码表示，同样不难发现，**小数 0 同样有两种表示方法。**

![image-20210321151342295](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321151342295.png)

我们再来看四个例子

![image-20210321151625733](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321151625733.png)

我们不难发现，对于小数，正小数的真值和原码表示的**格式** 是一致的，但是这里大家要注意，**二者小数点前面的 0 的含义确完全不同！** 

另外，由于原码表示是要存储在计算机中的，所以**想要求得某个真值的原码表示，必须已知机器数的数值部分的长度才行。** 

我们再来一起做几组练习

![image-20210321152527982](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321152527982.png)



![image-20210321152543052](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321152543052.png)

![image-20210321152633428](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321152633428.png)

**通过最后一个例子，我们不难验证，$$x=0$$ 的原码有两个不同的值！**

最后，我们对原码做一个总结。

不难发现，原码表示法的特点是：**简单** 、**直观** ！但是，使用原码做加法运算的时候，会出现一些问题：

![image-20210321153033877](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321153033877.png)

![image-20210321155410369](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321155410369.png)

**这样不仅需要加法器，还需要减法器，同时结果的处理情况也比较复杂** ，我们不禁要想，能不能**只做加法** ?

具体做法是**找到一个与负数等价的正数来代替这个负数** ，从而将**减法运算变为加法运算** ！这就产生了**补码** 的概念。



### 3.补码表示法

#### 1.补的概念

我们都有过给时钟调整时间的经验，比如现在时钟表针指向 6 时，我想把它调整至 3 时，很容易想到**有两种不同的做法** 能够达到目标！

- 逆时针拨动 3 小时，记为 -3
- 顺时针拨动 9 小时，记为 +9

上面两种操作虽然不同，但是最终达到的结果是一致的。我们可以说**$$-3$$ 与$$+9$$ 在模 12 的条件下是相等的** （模12的意思就是只看余数）！仔细看一下这两个数字，**我们是不是已经成功地把负数$$-3$$ 用正数$$+9$$ 代替了？** 

具体可以看一下这张图片，其实这就是**补码** 的来历~

![image-20210321154539647](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321154539647.png)

不难发现，当一个正数和一个负数互补的时候，它们两个之和正好等于**模数** 。

在下图的例子中，我们考虑怎么样从$$1011$$ 运算得到 $$0000$$ ?

- 第一种方法，减去 1011
- 第二种方法，加上 0101，进位自然舍去

在上面的例子中，**-0100 的补数其实就是 +0101** 

![image-20210321160038033](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321160038033.png)

#### 2.一个小例子

首先，来看两个互为补数的整数$$-1011$$ 与$$+0101$$ ,换成二进制是 -11 和 +5，不难发现，这两个数是关于 16 互补的，也就是在 模16取余数的情况下是相等的。

![image-20210321164000111](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321164000111.png)

我们接下来的任务是把这两个真值变为补数表示，从而可以在计算机中存储。

对这两个数字分别加上模（16），得到下面的情况：

![image-20210321164214882](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321164214882.png)

注意，在第二个数字加上16后会溢出，我们直接把超过四位的数字扔掉，于是得到了 **+0101的补数是 0101** ，同时，细心的你一定也会发现，**-1011的补数也是0101**！，这就出现了问题！**0101到底是表示哪个真值？**

于是我们在得到的补数的**最前面再加上一位符号位**用来确认！而增加的这一位符号位的权值是$$2^n$$ ，也就相当于我们**又加了一个$$2^n$$ ** ,这下总算可以区分上面的两个真值了。

![image-20210321164735042](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321164735042.png)

注意在上面的过程中，我们**一共加了两次$$2^n$$ ** ,所以最后的结果就是**直接模$$2^{n+1}$$ ** ！



#### 3.补码的定义

##### 1.整数

通过前面的小例子，我们已经初步接触到了补码，现在给出具体的定义

![image-20210321165252461](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321165252461.png)

举个例子：

![image-20210321165319475](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321165319475.png)



##### 2.小数

小数补码的定义：

![image-20210321165352907](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321165352907.png)

举例：

![image-20210321165407032](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210321165407032.png)

**这里要注意机器数规定长度的限制！** 

另外，这里的补码处理是以模2进行的。如果模2，小数点前面1位；如果模4，小数点前面2位，都是可以的。

#### 4.求补码的快捷方式

我们考虑对$$x=-1010$$ 求补码表示，根据上面的定义公式，真值是4位，所以$$2^{4+1}$$ , 也就是如下图所示：

![image-20210322102953654](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322102953654.png)

所以补码表示就是$$1,0110$$ ,这一点是很清晰的！

从另一个角度来看：

![image-20210322103108208](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322103108208.png)

**用$$11111$$ 减去真值的绝对值，其实就是对每一位取反** ，然后再加上 1 就得到了补码表示。

所以，负数求补码的方法总结一下就是：

- 符号位不变
- 数值位取反加一

![image-20210322103412923](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322103412923.png)

#### 4.举例

![image-20210322103649702](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322103649702.png)

![image-20210322103748913](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322103748913.png)

![image-20210322104107096](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322104107096.png)

**注意一个小规律：从补码转换为原码的操作同样是：**

- **符号位不变**
- **数值位取反加一**

![image-20210322104134588](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322104134588.png)

![image-20210322105357782](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322105357782.png)



最后，补码最重要的是**能够把减法变成加法** ，这才是最重要的！

### 4.反码表示法

#### 1.整数定义

反码的定义相比补码要简单一点，对于正数，两者是一样的，对于负数，**仅仅是把补码要加的那个$$1$$ 不加了，单纯对数值位取反！** ，这样的话，用严格的数学定义，就是$$模2^{n+1}-1$$ 了。 

![image-20210322110028821](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322110028821.png)

来看两个例子：

![image-20210322110410168](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322110410168.png)

#### 2.小数定义

对于小数的反码定义，其实和整数一致，但是要注意**数值位最后面的那个 $$“1”$$  其实是$$2-2^{-n}$$ 。** 

![image-20210322110809265](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322110809265.png)

举例如下：

![image-20210322110824859](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322110824859.png)

再看几个小例子：

![image-20210322111155350](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322111155350.png)

![image-20210322111213364](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322111213364.png)

![image-20210322111228016](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322111228016.png)

**注意$$+0$$ 与$$-0$$ 的反码机器数表示是不同的** 



### 5.三种机器数的小结

![image-20210322111449599](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322111449599.png)

![image-20210322121501560](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322121501560.png)

三种机器数表示的范围如下（以8位机器数字长为例）：

![image-20210322112047873](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322112047873.png)

接下来我们再来看一个比较重要的结论：

![image-20210322112801834](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322112801834.png)

分两种情况讨论：y 为正数和 y 为 负数

![image-20210322112914357](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322112914357.png)

得到下面这个结论：

![image-20210322112904385](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322112904385.png)

对于第二种情况：

![image-20210322113054830](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322113054830.png)

得到下面结论：

![image-20210322113113947](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322113113947.png)

综上所述：**$$[y]_{补}$$ 连同符号位在内，每位取反，末位加$$1$$ ，即得到$$[-y]_{补}$$ ！** ，这是个十分重要的结论！

### 6.移码表示法

#### 1.移码定义

移码的提出是为了解决**补码表示很难直接判断真值大小**这样一个问题。举例如下：

![image-20210322120211382](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322120211382.png)

因为在补码表示中，负数第一个符号位是 1，正数第一个符号位是 0 ，而$$1>0$$ ,这样直接使用补码进行大小判断得到的结果是不可靠的，所以我们引进了移码的表示方法。**通过对真值加上特定的数字进行“平移”，从而解决这一问题** 

![image-20210322120428068](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322120428068.png)

接来我们给出移码的定义：

![image-20210322120501819](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322120501819.png)

简单来说，**移码中的“移”就是在数轴上进行平移** 

#### 2.移码和补码的比较

![image-20210322120618799](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322120618799.png)

结合示例很容易发现**补码和移码只差一个符号位** ，从而完美的解决了比较大小的问题。

#### 3.真值、补码和移码的对照表

![image-20210322120654937](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322120654937.png)

#### 4.移码的特点

![image-20210322120747560](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322120747560.png)

另外两点值得注意的是：

- **在移码的平移过程中，最小的真值被移动到了位置 0**
- **移码主要用在表示浮点数的阶码**

