---
layout:     post
title: "三十张图助你看清红黑树的前世今生"
date: 2020-07-05
author: "孙志超"
catalog: false
header-style: text
tags:
  - 算法
---

在《算法》（第4版）中，红黑树的实现直接采用了左倾红黑树 (LLRB) 的方法，左倾红黑树可以用更少的代码量实现红黑树，在本文中我也使用他的方法理解。相比于经典红黑树，增加了一个限制**红节点一定是父节点的左子节点**，但是实现却容易不少

<!--more-->
## 一、红黑树的定义

1.每个节点要么是黑色要么是红色；

2.根节点是黑色；

3.每个叶子节点都是黑色的空节点（NIL），也就是说，叶子节点不存储数据；

4.任何相邻的节点都不能同时为红色，也就是说，红色节点是被黑色节点隔开的；

5.每个节点，从该节点到达其可达叶子节点的所有路径，都包含相同数目的黑色节点；

这就是红黑树的定义，但你看完肯定会一脸懵，我也是一样。我会想：红黑树的来源是什么？为什么要区分红色和黑色节点呢？这些性质是怎么来的，或者有什么作用？不着急，你听我慢慢道来，我希望，你能够通过这篇文章对红黑树有一个清晰的认识，包括它的来历，意义以及各种操作。

## 二、平衡二叉查找树

首先，还记得咱们上次文章中介绍的**二叉查找树**吗？尽管它具有不错的性能，但是仍然无法避免极端的情况。一般的二叉查找树的生成和数据插入的顺序密切相关，我们来看一组情况：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731802153e8c2da)
我们依次插入[B,A,C,G,R,Z]，最后得到的树无疑已经退化到接近链表，这时，性能会受到很大地影响。
我们的任务就是**试图构造一种新的数据结构**来解决这种问题，即构建**平衡二叉查找树**：


![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17318346b677390f)

平衡二叉查找树的严格定义是：每个节点的左子树和右子树的高度差至多等于1。但我们不必严格死扣定义，我们只需要知道，平衡二叉查找树中“平衡”的意思，其实就是让整棵树左右看起来比较“**对称**”、比较“**平衡**”，不要出现左子树很高、右子树很矮的情况。这样就能让整棵树的高度相对来说低一些，相应的插入、删除、查找等操作的效率高一些。

## 三、2-3树

我们不着急直接介绍红黑树，请跟随我的思路先学习一下`2-3树`，循序渐进地走向红黑树。

### 2-3树的定义
> 一棵2-3树由以下节点组成：
> - 2-节点，含有一个键（及其对应的值）和两条链接，左连接指向的2-3树中的键都小于该节点，右链接指向的2-3树中的键都大于该节点。
> - 3-节点，含有两个键（及其对应的值）和三条链接，左链接指向的2-3树中的键都小于该节点，中链接指向的2-3树中的键都位于该节点的两个键之间，右链接指向的2-3树中的键都大于该节点。


![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17318a91398fc78d)

如果将一棵`2-3树`进行中序遍历，得到的是一个有序的序列，比如我们对下图进行中序遍历，会得到`[A,C,F,J,K,M,O,Q,R,S,T]`。

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17318978c9b947ab)

### 2-3树的创建

我们首先给出两条原则【融合】与【拆分】：
- 原则1. 加入新节点时，不会往空的位置添加节点，而是添加到最后一个叶子节点上(使2-节点变为3-节点，3-节点变为4-节点)

- 原则2. 如果出现4-节点，要将它分解成三个2-节点组成的树，并且分解后新树的根节点需要向上和父节点融合（父节点变成3-节点或者4-节点）

接下来我们一起看一下`2-3`树的生长过程：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731914b21bbe716)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173191520db9d59c)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731915954012c17)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731915d732073b4)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731916245c5db87)

可以发现，虽然我们的插入顺序是`[A->C->F->J->K->M->O->]`,是升序的，但是我们构造的2-3树却始终保持平衡。

那么，如何实现这种高效的**数据结构**呢？上面的一些操作我们描述清楚是一回事，但是实现又是另外一回事。我们选择**红黑二叉查找树**这种数据结构来实现它，简称**红黑树**。

### 红黑树与2-3树的等价

红黑树背后的思想是**用标准的二叉查找树（完全由2-节点构成）和一些额外的信息（替换3-节点）来表示2-3树**。我们将树中的链接分为两种类型：一种是红链接（左倾），一种是黑链接。其中红链接将两个2-节点连接起来构成一个3-节点，黑节点则是2-3树中的普通节点。为了方便，我们把**红链接指向的节点标记为红色，其他节点标记为黑色**，这就是红黑树的由来，具体见下图所示：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731947d5d880f19)

我们再重新审视一下红黑树定义要求的各种性质：

（1）每个节点要么是黑色要么是红色；（这个就不多说了，很自然）

（2）根节点是黑色；

  根节点只有两种情况，第一种可能是`2-节点`，此时对应着黑色；第二种可能是`3-节点`,此时根节点也是黑色的，你可以结合上图理解。

（3）每个叶子节点都是黑色的空节点（NIL），也就是说，叶子节点不存储数据；

 这一条主要是为了简化红黑树的实现而设置的，严格来说，我们之前画的还不完整。


![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731990694d738c6)

（4）任何相邻的节点都不能同时为红色，也就是说，红色节点是被黑色节点隔开的；

我们假设存在同时为红色的两个相邻节点，那么会怎么样呢？见下图：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173199342b67ca70)

（5）每个节点，从该节点到达其可达叶子节点的所有路径，都包含相同数目的黑色节点；

这条要求保证了红黑树的性能。我们随意选一条从根节点到叶子节点的路径，由于性质4，任何相邻的节点不能同时为红色，所以每存在一个红节点，至少对应了一个黑节点，即**红色节点个数<=黑色节点个数**，我们再结合`2-3树`，每个黑色节点对应着一个2-节点或一个3-节点；根据`2-3树`的性质，**其节点<=log(N)**,可以推出**黑色节点<=log(N)**,那么**加上红色节点不会大于2log(N)**。

总结如下就是：
> 红色节点个数<=黑色节点个数=====>每一个黑色节点对应2-3树的每一个节点====>2-3树节点<=log(N)====>黑色节点<=log(N)====>红黑树节点<=2log(N)

> 我们将2-3树染色后称为红黑树，然后给出了**一些颜色的规则**，这些规则能够帮助我们快速的构建使用红黑树。这使得我们以后只需要将注意力集中在**颜色**上面，只需要去**维护上述规定的性质**即可。我想，这大概就是红黑树存在的意义！

红黑树的节点表示代码如下：

```java
private static final boolean RED = true;
private static final boolean BLACK = false;

private class Node{
    int key;//键
    String value;//值
    Node left,right;//左右子树
    boolean color;
}

//构造函数
Node(int key,String value,boolean color){
    this.key=key;
    this.value=value;
    this.color=color;
}

//判断节点x的颜色
private boolean isRed(Node x){
    if(x==null) return false;
    return x.color==RED;
}
```

## 四、红黑树的创建

首先，我们回顾一下2-3树的创建过程，
- 原则1. 加入新节点时，不会往空的位置添加节点，而是添加到最后一个叶子节点上(使2-节点变为3-节点，3-节点变为4-节点)

- 原则2. 如果出现4-节点，要将它分解成三个2-节点组成的树，并且分解后新树的根节点需要向上和父节点融合（父节点变成3-节点或者4-节点）

原则1我们可以对应为：每次插入新节点的时候都插入红色节点+根节点永远是黑色。这是因为红色节点被红色链接所指向，而红色链接是2-3节点的**内部链接**，或者也可以理解为**始终插入的位置都是同级节点**。另一方面，初始化树的时候，第一个节点应该是黑色的，所以增加这一条规则。

原则2我们可以对应为三种简单的操作：**左旋转，右旋转和改变颜色**

所以，综合以上对应关系，我们可以**直接创建红黑树而不必考虑2-3树的种种性质**。

- 1、保持根节点是黑色
- 2、左旋转
- 3、右旋转
- 4、改变颜色


#### 1.左旋转[围绕某个节点左旋转]

在我们定义的红黑树中，**红色链接永远是左链接**，换句话说，**红色节点永远是父节点的左子节点**，所以，在在操作的过程中，为了维持这种性质，我们实现了左旋转这个操作。

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731cc021737ec5e)

功能：右链接变左链接

具体过程见图片：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731a45654e09271)



我们只是将用两个键中较小的那个作为根节点改变为将较大者作为根节点，代码的实现还是比较容易的，结合上图理解更轻松。
```java
Node rotateLeft(Node h){
    Node x=h.right;
    h.right=x.left;
    x.left=h;
    h.color=RED;
    return x;
}
```

#### 2.右旋转[围绕某个节点右旋转]

右旋转其实和左旋转完全类似，它的功能是：左链接变右链接，代码只需要将`left`和`right`互换即可。

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731cc068db27757)
![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731c63cefa09893)

代码如下:
```java
Node rotateRight(Node h){
    Node x=h.left;
    h.left=x.right;
    x.right=h;
    x.color=RED;
    return x;
}
```
#### 3.改变颜色


![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731e630483cb191)

```java
void flipColors(Node h){
    h.color=RED;
    h.left.color=BLACK;
    h.right.color=BLACK;
}
```

> 如果你已经看到这里了，我先给你树个大拇指👍，接下来更要打起精神来！
## 五、红黑树的插入

我们先给出所有可能的情况，心中有一个总体的大纲，注意：**我们总是插入红色节点**

1、插入的节点父亲为黑色（2-节点）

2、插入的节点父亲为红色（3-节点）
- 插入的节点的键大于3-节点的两个键
- 插入的节点的键小于3-节点的两个键
- 插入的节点的键介于3-节点的两个键之间

针对以上所有情况，我们的做法就是**通过左旋转、右旋转、改变颜色来使得被破坏的红黑树的性质得以恢复**

第一种情况：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731e234342bf329)

第二种情况：

（1）新键最大

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731e317e50c0d0a)

（2）新键最小

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731e3f3619a470c)

（3）新键介于两者之间

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731e498d3621ca3)


总结一下，我们只要谨慎地使用左旋转、右旋转和改变颜色这三个简单的操作。我们就能保证插入操作后红黑树和2-3树的一一对应关系。在沿着插入点到根节点的路径向上移动时所经过的每个节点中顺序的完成以下操作，我们就能完成插入操作。

- 如果右子节点是红色而左子节点是黑色，进行左旋转
- 如果左子节点是红色且它的左子节点也是红色，进行右旋转
- 如果左右子节点均为红色，改变颜色

以下，我们看一下具体的代码实现：

```java
public class LLRBTree{
    private Node root;
    private class Node;//见前文代码部分
    
    //见前文
    private boolean isRed(Node h);
    private Node rotateLeft(Node h);
    private Node rotateRight(Node h);
    private void flipColors(Node h);
    
    public void put(int key,String value){
        //查找key，找到则更新它的值，找不到就创建一个新的节点
        root=put(root,key,value);
        root.color=BLACK;//根节点永远是黑色的
    }
    
    private Node put(Node h,int key,String value){
        if(h==null) //标准的插入操作，和父节点用红链接相连
            return new Node(key,value,RED);
        if(key<h.key) h.left=put(h.left,key,value);
        else if(key>h.key) h.right=put(h.right,key,value);
        else h.value=value;
        
        if(isRed(h.right)&&!isRed(h.left)) h=rotateLeft(h);
        if(isRed(h.left)&&isRed(h.left.left) h=rotateRight(h);
        if(isRed(h.left)&&isRed(h.right)) flipColors(h);
        
        return h;
    }
}
```
到现在为止，红黑树的插入操作已经完成了，如果哪里不明白，仔细想想，或者你可以翻阅以下《算法》（第四版）。加油！
## 六、红黑树的删除

对于经典的红黑树来说，删除操作涉及的情况分类比较多和复杂，但是，左倾红黑树（LLRB）简化了这一操作。我们按照以下三步去讲解：**注意，我们会使用4-节点（三个键，4个链接）去理解**
- 删除最小键
- 删除最大键
- 删除任意键

### 1.删除最小键

- 1.递归地搜索左子树，直到找到做左边的元素（即最小键）
- 2.如果搜索结束于一个3-节点或一个4-节点，直接把最小键删除即可
- 3.如果结束于2-节点，删除2-节点（黑节点）会破坏红黑树的性质
    - 沿着搜索路径改变树的节点
    - 如果当前的节点不是2-节点就不改变

对于第2条，如果结束于3-节点或者4-节点，说明最小键是红节点，直接删除红节点不会改变红黑树的性质。第三条说的就是解决方案，**我们通过一些调整使得最小键处于一个3-节点或4-节点上**。

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731e88d8bd8328e)

左边是原始的结束于2-节点的情况，右边是我们希望的状态。

我们的操作是**“将一个红链接一直搬运到最左端”**。

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731ea10d6191950)

在`h`从根节点沿着路径下移的过程中，如果`h.left`或者`h.left.left`中有一个是红节点，就不做调整；当且仅当`h.left`和`h.left.left`都是黑色的时候，我们做调整（制造红色节点）。

这又分为两种情况：`h.right.left.color=BLACK`和`h.right.left.color=RED`

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731eb13990f1876)

具体的实现代码：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731eb602bf26392)

样例图示：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731eb737c5a64db)

### 2.删除最大键
和删除最小键同样的道理，具体的代码如下：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731eb8afb9ce2b0)

样例图示：

![](https://user-gold-cdn.xitu.io/2020/7/5/1731eb9761ec6a98?w=1133&h=859&f=png&s=336885)

小超在这里除了感觉比较巧妙与神奇外没有任何想法:(233   但是，毫无疑问，代码的实现是简洁与优美的。

### 3.删除任意键

有了前面两个函数做支撑，我们可以很轻松的实现删除任意键。

具体做法：用待删除点的后继节点代替该节点，然后删除后继节点（deleteMin(h.right)。这里为什么用到后继节点呢？前驱和后继节点是与待删除节点最近的节点，改变它们的位置只会影响整个树的一小部分（一个局部），从而我们可以较为轻松地恢复红黑树的性质。请看下面这张图：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731ef8a6d0790f7)

我们在递归向下的过程中始终保证一点：`节点h`或者`节点h的孩子节点`其中至少有一个是红节点。
- 沿着路经向左搜索：使用函数 `moveRedLeft()`
- 沿着路径向右搜索：使用函数`moveRedRight()`
- 找到后在底部删除节点
- 沿着原路径返回时修复之前产生的**右链接**

具体代码：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/1731efe3ed916eda)

总结一下，我们的删除最大键和删除最小键的算法，其实是结合2-3-4树的一些性质，在删除之前通过一系列操作**为删除创造条件**，而这个条件一旦满足后，我们就可以在不产生任何后果的情况下进行删除操作。删除前的一系列铺垫小超这里觉得实在是**巧妙**😲，我觉得，你根据后面的这些配图和代码应该可以理解他的思路，毕竟，情况不多！左倾红黑树（LLRBTree）大大减少了需要分类讨论的情况。这是一项伟大的发明！

>推荐大家去看 **《算法4》（后台回复【算法4】即可）**，另外，英语不错的话推荐看参考中的两篇文章（一篇论文，一篇PPT）

参考:  
[1]《算法》（第四版）
    
[2][Left-leaning Red-Black Trees Robert Sedgewick](https://www.cs.princeton.edu/~rs/talks/LLRB/LLRB.pdf)

[3][Left-Leaning Red-Black Trees](https://www.cs.princeton.edu/~rs/talks/LLRB/08Dagstuhl/RedBlack.pdf)

全文完