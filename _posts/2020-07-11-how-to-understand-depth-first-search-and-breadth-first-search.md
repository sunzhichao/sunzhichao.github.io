---
layout:     post
title: "深度优先搜索与广度优先搜索"
date: 2020-07-11
author: "孙志超"
catalog: false
header-style: text
tags:
  - 算法
---
27张图，万字长文，紧紧追踪算法的每一步！深搜与广搜如何理解？它们又有哪些重要的应用呢？本文全部告诉你~
<!--more-->

> 约定：本文所有涉及的图均为无向图，有向图会在之后的文章设计

## 1.图的存储方式
我们首先来回顾一下图的存储方式：邻接矩阵和邻接表。为了实现更好的性能，我们在实际应用中一般使用`邻接表`的方式来表示图。


![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173371f3e0632a9c)
具体的实现代码为：

```java
package Graph;

import java.util.LinkedList;

public class Graph{
    private final int V;//顶点数目
    private int E;//边的数目
    private LinkedList<Integer> adj[];//邻接表

    public Graph(int V){
        //创建邻接表
        //将所有链表初始化为空
        this.V=V;this.E=0;
        adj=new LinkedList[V];
        for(int v=0;v<V;++v){
            adj[v]=new LinkedList<>();
        }
    }

    public int V(){ return V;}//获取顶点数目
    public int E(){ return E;}//获取边的数目

    public void addEdge(int v,int w){
        adj[v].add(w);//将w添加到v的链表中
        adj[w].add(v);//将v添加到w的链表中
        E++;
    }

    public Iterable<Integer> adj(int v){
        //我们不必注意这个细节，可以直接把它忽视而不会影响任何关于图的理解与实现
        return adj[v];
    }

}
```
接下来，我们会首先介绍`深度优先搜索`与`广度优先搜索`的原理和具体实现；然后根据这两个基本的模型，我们会介绍六种典型的应用，这些应用只是在深搜和广搜的代码的基础上进行了一些加工，却可以解决不同的问题！

**注意：我们的思路：如何遍历一张图？==>深搜与广搜==>能够解决的问题。**

## 2.深度优先搜索

深度优先搜索是利用递归方法实现的。我们只需要在访问其中一个顶点时：
- 将它标记为已经访问
- 递归地访问它的所有没有被标记过的邻居顶点

我们来仔细地看一下这个过程：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b61cbb9cdbc)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b66ee27a447)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b6b45703150)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b70367d6caf)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b746f3e1881)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b7cbba5c05f)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b816eff01d6)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b85ad9f5f05)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b89c3ab6c30)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b8d79ec7f4a)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b91f33c0460)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337b95669ac484)

深度优先搜素大致可以这样描述：不撞南墙不回头，它沿着一条路一直走下去，直到走不动了然后返回上一个岔路口选择另一条路继续前进，一直如此，直到走完所有能够到达的地方！它的名字中**深度**两个字其实就说明了一切，每一次遍历都是走到最深！

注意一个细节：在我们上面的最后一个图中，顶点3仍然未被标记（绿色）。我们可以得到以下结论：

**使用深度优先搜索遍历一个图，我们可以遍历的范围是所有和起点连通的部分**，通过这一个结论，下文我们可以实现一个**判断整个图连通性**的方法。


深度优先搜索的代码实现，我是用Java实现的，其中，我定义了一个类，这样做的目的是更加清晰（毕竟，一会后面还有很多算法）

```java
package Graph;

public class DepthFirthSearch {

    private boolean[] marked;//用来标记顶点

    public DepthFirthSearch(Graph G,int s){
        //s是起点
        marked = new boolean[G.V()];
        dfs(G,s);
    }

    private void dfs(Graph G,int v){
        marked[v]=true;//标记顶点，这是我们的第一条原则
        
        //对于所有没有被标记的邻居顶点，递归访问，这时第二条原则
        for(int w:G.adj(v))
            if(!marked[w]) dfs(G, w);
    }

    public boolean marked(int w){
        //判断一个顶点能否从起点到达；因为在深搜的过程中，只要被标记了就是能够到达，反正就是不连通的
        return marked[w];
    }

    
}
```

## 3.深搜应用(一):查找图中的路径

我们通过深度优先搜索可以轻松地遍历一个图，如果我们在此基础上增加一些代码就可以很方便地查找图中的路径！

比如，题目给定`顶点A`和`顶点B`,让你求得从`A`能不能到达`B`？如果能，给出一个可行的路径！

为了解决这个问题，我们添加了一个实例变量`edgeTo[]`整型数组来记录路径。比如：我们从顶点`A`直接到达`顶点B`,那么就令`edgeTo[B]=A`，也就是“**edge to B is A**”，其中`A`是距离`B`最近的顶点且从`A`可以到达`B`。我举个简单的例子：

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17337e78cbb84b76)

具体的代码如下，其中为了实现这个功能，我定义了一个完整的类：
```java
package Graph;

import java.util.Stack;

public class DepthFirstPaths {

    private boolean [] marked;//记录是否已经访问
    private int[] edgeTo;//从起点到一个顶点的已知路径上的最后一个顶点
    private final int s;//查找的起点

    public DepthFirstPaths(Graph G,int s){
        //在图G中查找，s是起点
        marked = new boolean[G.V()];
        edgeTo = new  int[G.V()];
        this.s=s;
        dfs(G,s);//递归调用dfs
    }

    private void dfs(Graph G,int v){
        //从起点v开始查询
        marked[v]=true;
        for(int w:G.adj(v)){
            if(!marked[w]){
                edgeTo[w]=v;//w的前一个顶点是v
                dfs(G,w);//既然w没有被标记，就递归地进行dfs遍历它
            }
        }

    }

    public boolean hasPathTo(int v){
        //判断是否有从起点到顶点v的路径
        //如果顶点v被标记了，就说明它可以到达，否则，就不可以到达
        return marked[v];
    }

    //打印路径
    public void pathTo(int v){
        if(!hasPathTo(v)) System.out.println("不存在路径");;
        Stack<Integer> path=new Stack<Integer>();

        for(int x=v;x!=s;x=edgeTo[x]){
            path.push(x);
        }
        path.push(s);
        //打印栈中的元素
        while(path.empty()==false)
            System.out.print(path.pop()+"  ");
        System.out.println();
    }

    
}
```

最后一个打印函数`pathTo`地思想就是通过一个`for`循环，将路径压到一个栈里，通过栈地先进后出地性质实现反序输出。理解了上面的`dfs`地过程就好，这里可以单独拿出来去理解。

## 4.深搜应用(二)：寻找连通分量

还记得我们上文讲到的`dfs`的一条性质吗？**一个dfs搜索能够遍历与起点相连通的所有顶点**。我们可以这样思考：申请一个整型数组`id[0]`用来将顶点分类——“联通的顶点的`id`相同，不连通的顶点的`id`不同”。首先，我对顶点`adj[0]`进行`dfs`，把所有能够遍历到的顶点的`id`设置为`0`，然后把这些顶点都标记；接下来对所有没有被标记的顶点进行`dfs`，执行同样的操作，比如将`id`设为`1`，这样依次类推，直到把所有的顶点标记。最后我们我们得到的`id[]`就可以完整的反映这个图的连通情况。

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173381dac2b8f250)

如果我们需要判断两个顶点之间是否连通，只需要比较它们的`id`即可；同时，我们还可以根据有多少个不同的`id`来获得一个图中`连通分量`的个数，一举两得！

具体的代码实现如下：
```java
package Graph;

public class CC {

    private boolean[] marked;
    private int [] id;//用于记录连通信息，相当于身份id
    private int count;//用来判断最终一共有多少个不同的id值

    public CC(Graph G){
        marked = new boolean[G.V()];
        id=new int[G.V()];
        
        for(int s=0;s<G.V();s++){
            if(!marked[s]){
                dfs(G,s);
                count++;
            }
        }
    }
    
    //以下就是一次完整的dfs搜索，它所遍历的顶点都是连通的，对应了同一个count
    private void dfs(Graph G,int v){
        marked[v]=true;
        id[v]=count;
        for(int w:G.adj(v)){
            if(!marked[w])
                dfs(G,w);
        }
    }
    
    //判断两个顶点是否联通
    public boolean connected(int v,int w){
        return id[v]==id[w];
    }
    
    //返回身份id
    public int id(int v){
        return id[v];
    }

    //返回一个图中连通分量的个数，也就是一共有多少个身份id
    public int count(){
        return count;
    }
}
```

## 5.深搜应用(三):判断是否有环

## 6.深搜应用(四):判断是否为二分图

二分图定义是：能够用两种颜色将图的所有顶点着色，使得任意一条边的两个端点的颜色都不相同。

对于这个问题，我们依然只需要在`dfs`中增加很少的代码就可以实现。

具体思路：我们在进行`dfs`搜索的时候，**凡是碰到没有被标记的顶点时就将它依据二分图的定义标记（使相邻顶点的颜色不同），凡是碰到已经标记过的顶点，就检查相邻顶点是否不同色。**在这个过程中，如果发现存在相邻顶点同色，则不是二分图；如果直到遍历完也没有发现上述同色情况，则是二分图，且上述根据`dfs`遍历所染的颜色就是二分图的一种。

具体的代码实现:

```java
package Graph;

public class TwoColor{
    private boolean[] marked;
    private boolean[] color;//用布尔值代表颜色
    private boolean isTwoColorable = true;
    
    public TwoColor(Graph G){
        marked=new boolean[G.V()];
        color=new boolean[G.V()];
        for(int s=0;s<G.V();s++){
            if(!marked[s]){
                dfs(G,s);

            }
        }
    }

    private void dfs(Graph G,int v){
        marked[v]=true;
        for(int w:G.adj(v)){
            if(!marked[w]){
                color[w]=!color[v];//如果没有被标记，就按照二分图规则标记
                dfs(G,w);
            }
            else if(color[w]==color[v]) isTwoColorable=false;//如果被标记就检查
        }
    }

    public boolean isBipartite(){
        return isTwoColorable;
    }
}
```
## 5.广度优先搜索

在很多情境下，我们不是希望单纯地找到一条连通的路径，而是**希望找到最短的那条**，这个时候深搜就不再发挥作用了，我们接下来介绍另一种图的遍历方式：广度优先搜索（bfs）。我们先介绍它的实现，然后再介绍如何寻找最短路径。

广度优先搜索使用一个队列来保存所有已经被标记过但其邻接表还未被检查过的顶点。它先将起点加入队列，然后重复以下步骤直到队列为空。
- 取队列中的第一个顶点`v`出队
- 将与`v`相邻的所有未被标记过的顶点先标记后加入队列

注意：在广度优先搜索中，我们并没有使用递归，在深搜中我们隐式地使用栈，而在广搜中我们显式地使用队列

一起来看一下具体的过程吧~

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173385e699700c8a)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173385eb0a4c63f8)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173385eed6e5ff19)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173385f2dfcd256d)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173385f682603e09)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173385f9b9f691ab)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/173385fd47fecf90)

![](https://tuchuang-01.oss-cn-beijing.aliyuncs.com/img/17338600ec954349)

直观地讲，它其实就是一种“地毯式”层层推进的搜索策略，即先查找离起始顶点最近的，然后是次近的，依次往外搜索。

我相信你在仔细追踪了上图后对广度优先搜索有了一个完整的认识，那么接下来我就附上具体的代码实现：
```java
package Graph;

import java.util.LinkedList;
import java.util.Queue;

public class BreadthFirstSearch {

    private boolean[] marked;
    private final int s;//起点

    public BreadthFirstSearch(Graph G,int s){
        marked = new boolean[G.V()];
        this.s=s;
        bfs(G,s);
    }

    private void bfs(Graph G,int v){

        Queue<Integer> queue = new LinkedList<>();
        marked[v]=true;//标记queue起点
        queue.add(v);//将起点加入队列
        while(!queue.isEmpty()){
            int t=queue.poll();//从队列中删去下一个顶点
            for(int w:G.adj(t)){
                if(!marked(w)){
                    //对于每个没有被标记的相邻顶点
                    marked[w]=true;//标记它
                    queue.add(w);//并将它添加到队列
                }
            }
        }

    }
    public boolean marked(int w){
        return marked[w];
    }
}
```
## 6.广搜应用(一):查找最短路径
其实只要在广度优先搜索的过程中添加一个整型数组`edgeTo[]`用来存储走过的路径就可以轻松实现查找最短路径，因为其原理和广搜中的`edgeTo[]`完全一致，所以这里我就不多说了。

以下是具体的代码实现：
```java
package Graph;

import java.util.LinkedList;
import java.util.Queue;
import java.util.Stack;

public class BreadthFirstPaths {

    private boolean[] marked;
    private int[] edgeTo;//到达该顶点的已知路径上的最后一个顶点
    private final int s;//起点

    public BreadthFirstPaths(Graph G,int s){
        marked = new boolean[G.V()];
        edgeTo = new int[G.V()];
        this.s=s;
        bfs(G,s);
    }

    private void bfs(Graph G,int v){

        Queue<Integer> queue = new LinkedList<>();
        marked[v]=true;//标记queue起点
        queue.add(v);//将起点加入队列
        while(!queue.isEmpty()){
            int t=queue.poll();//从队列中删去下一个顶点
            for(int w:G.adj(t)){
                if(!marked(w)){
                    edgeTo[w]=t;//保存最短路径的最后一条边
                    //对于每个没有被标记的相邻顶点
                    marked[w]=true;//标记它
                    queue.add(w);//并将它添加到队列
                }
            }
        }

    }
    public boolean marked(int w){
        return marked[w];
    }

    
    public boolean hasPathTo(int v){
        //判断是否有从起点到顶点v的路径
        //如果顶点v被标记了，就说明它可以到达，否则，就不可以到达
        return marked[v];
    }

    public void pathTo(int v){
        if(!hasPathTo(v)) System.out.println("不存在路径");;
        Stack<Integer> path=new Stack<Integer>();

        for(int x=v;x!=s;x=edgeTo[x]){
            path.push(x);
        }
        path.push(s);
        //打印栈中的元素
        while(path.empty()==false)
            System.out.print(path.pop()+"  ");
        System.out.println();
        
    }
}
```

## 7.广搜应用(二)：求任意两顶点间最小距离

设想这样一个问题：给定图中任意两点（u，v），求解它们之间间隔的最小边数。

我们的想法是这样的：以其中一个顶点（比如u）为起点，执行`bfs`，同时申请一个整型数组`distance[]`用来记录`bfs`遍历到的每一个顶点到起点u的最小距离。

**关键：假设在`bfs`期间，顶点x从队列中弹出，并且此时我们会将所有相邻的未访问顶点i{i1,i2……}推回到队列中，同时我们应该更新`distance [i] = distance [x] + 1;`**。它们之间的距离差为1。**我们只需要在每一次执行上述进出队列的时候执行这个递推关系式，就能保证`distance[]`中记录的距离值是正确的！！**

请务必仔细思考这个过程，我们很高兴只要保证这一点，就可以达到我们计算最短距离的目的。

以下是我们的代码实现：
```java
package Graph;

import java.util.LinkedList;
import java.util.Queue;

public class getDistance {

    private boolean[] marked;
    private int [] distance;//用来记录各个顶点到起点的距离
    private final int s;//起点

    public getDistance(Graph G,int s){
        marked = new boolean[G.V()];
        distance= new int[G.V()];
        this.s=s;
        bfs(G,s);
    }

    private void bfs(Graph G,int v){

        Queue<Integer> queue = new LinkedList<>();
        marked[v]=true;//标记queue起点
        queue.add(v);//将起点加入队列
        while(!queue.isEmpty()){
            int t=queue.poll();//从队列中删去下一个顶点
            for(int w:G.adj(t)){
                if(!marked(w)){
                    //对于每个没有被标记的相邻顶点
                    marked[w]=true;//标记它
                    queue.add(w);//并将它添加到队列
                    distance[w]=distance[t]+1;//这里就是需要添加递推关系的地方！
                }
            }
        }

    }
    public boolean marked(int w){
        return marked[w];
    
    //打印，对于一个给定的顶点，我们可以获得距离它特定长度的顶点
    public void PrintVertexOfDistance(Graph G,int x){
        for(int i=0;i<G.V();i++){
            if(distance[i]==x){
                System.out.print(i+" ");
            }
        }
        System.out.println();

    }

    
}
```
通过上面的方法，我们可以很容易的实现**求一个人的三度好友**之类的问题，我专门写了一个打印的函数（在上面代码片段最后），它接收一个整型变量`int v`,可以打印出所有到起点距离为`v`的顶点。

全文完