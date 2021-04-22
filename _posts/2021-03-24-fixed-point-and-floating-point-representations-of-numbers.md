---
layout:     post
title: "计算机组成：数的定点表示和浮点表示"
date: 2020-03-24
author: "孙志超"
catalog: false
header-style: text
tags:
  - 计算机组成原理
---

# 数的定点表示和浮点表示

![image-20210322134532726](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322134532726.png)

在计算机中，小数的表示**都是按照约定方式** 标出的，而根据**约定方式的不同** ，我们可以将其分为**定点表示** 和**浮点表示** 。

## 1.定点表示

定点表示，也就是小数点的位置是固定的。

![image-20210322134758983](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/image-20210322134758983.png)

根据小数点的定点位置不同，计算机可以分为两类：**小数定点机**和**整数定点机** ，它们的表示范围如图所示：

![image-20210322135631382](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210322135631382.png)

## 2.浮点表示

![image-20210322135721661](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210322135721661.png)

### 1.为什么要引入浮点数表示

![image-20210322140139216](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210322140139216.png)

### 2.浮点表示

浮点表示的形式其实就是我们日常生活中的**科学计数法** ，注意，阶码的表示是**二进制** ，尾数的基数一般取$$2,4,8,16$$ 。

![image-20210322140546494](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210322140546494.png)

#### 1.浮点数的表示形式

![image-20210322141155942](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210322141155942.png)

#### 2.浮点数的表示范围

![image-20210322141556971](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210322141556971.png)

看个例子：

因为机器数字长为24位是固定的，阶码位最小是$$m=4$$ ，而题目要求保证数的最大精度，最大精度是由尾数部分决定的，所以要求尾数尽量长，也就是阶码位尽量短，取 4 ，可得结论。

![image-20210323203211473](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210323203211473.png)

#### 3.浮点数的规格化形式

![image-20210323203725522](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210323203725522.png)

![image-20210323203738962](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210323203738962.png)

![image-20210323205343181](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210323205343181.png)

 ![image-20210323210748259](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210323210748259.png)

![image-20210323211854231](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210323211854231.png) 

![image-20210324082757638](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210324082757638.png)

**在补码表示中，只要尾数为全零或者阶数为全零，真值就是机器0** 

![image-20210324083116501](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210324083116501.png)![image-20210324083116530](C:\Users\ZhiChao\AppData\Roaming\Typora\typora-user-images\image-20210324083116530.png)