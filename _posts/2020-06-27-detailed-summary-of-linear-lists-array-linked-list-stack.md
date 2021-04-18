---
layout:     post
title: "线性表（数组、链表、队列、栈）详细总结"
date: 2020-06-27
author: "孙志超"
catalog: false
header-style: text
tags:
  - 算法
---

线性表是一种十分基础且重要的数据结构，它主要包括以下内容：

- 数组
- 链表
- 队列
- 栈

接下来，我将对这四种数据结构做一个详细的总结，其中对链表实现了**十几种**常见的操作。希望对你有所帮助。
<!--more-->

## 1.数组

**数组(Array)是一种线性表数据结构。它用一组连续的内存空间，来存储一组具有相同类型的数据。**
注意点：①.数组是一种**线性表**；②.**连续的内存空间和相同类型的数据**
由于第二个性质，数组支持 **“随机访问”**，根据下表随机访问的时间复杂度为O(1);但与此同时却使得在数组中删除，插入数据需要大量的数据搬移工作。

### 低效的“插入”和“删除”

#### 插入操作

假如数组的长度为n，我们需要将一个数据插入到数组的第k个位置，我们需要将第k~n位元素都顺序地往后挪动一位。
最好情况时间复杂度为O(1),此时对应着在数组末尾插入元素;
最坏情况时间复杂度为O(n),此时对应着在数组开头插入元素;
平均情况时间复杂度为O(n),因为我们在每个位置插入元素的概率相同，故(1+2+3+……+n)/n=O(n);
但是根据我们的需求，有一个特定的场景。如果数组的数据是有序的，那么我们在插入时就一定要那么做；但是如果数组中存储的数据并没有任何规律，数组只是被当成一个**存储数据的集合**，我们可以有一个取巧的方法：
直接将第k个元素搬移到数组元素的最后，把新的数据直接放入第k个位置即可（是不是很简单啊），这时插入元素的复杂度为O(1)。

#### 删除操作

和插入操作一样，为了保证内存的连续性，删除操作也需要搬移数据。
最好情况时间复杂度为O(1),此时对应着删除数组末尾的元素;
最坏情况时间复杂度为O(n),此时对应着删除数组开头的元素;
平均情况时间复杂度为O(n),因为我们删除每个位置的元素的概率相同，故(1+2+3+……+n)/n=O(n);
当然，在某些特殊情况下，我们并不一定非要进行**复杂**的删除操作。我们只是将需要删除的数据**记录**，并且**假装它以经被删除了**。直到数组没有更多空间存储数据时，我们再触发一次真正的删除操作即可。

>这其实就和生活中的垃圾桶类似，垃圾并没有消失，只是被“**标记**”成了垃圾，而直到垃圾桶塞满时，才会清理垃圾桶。

### 警惕数组访问越界

在 C 语言中，只要不是访问受限的内存，所有的内存空间都是可以自由访问的。如果疏忽会造成严重的后果。当然，Java会自动检测。

## 2.链表

- 链表结点表示
- 打印单链表
- 单链表根据索引插入结点
- 获取单链表的长度
- 打印单链表的长度
- 单链表删除指定索引的结点
- 单链表实现元素查找，返回是否存在布尔值
- 单链表删除指定索引的后续节点
- 单链表反转
- 递归地进行单链表反转
- 检测链表中是否含有环
- 删除倒数第k个结点
- 求中间节点
- 有序链表合并

### **链表结点表示**

```java
public class Node{
    int data;
    Node Next;
}
```

### **打印单链表**

```java
public class Method {
    //打印单链表
    public static void PrintNode (Node list){
        for(Node x=list;x!=null;x=x.Next)
        System.out.print(x.data+" ");
        System.out.println();
    }
```

### **单链表根据索引插入结点**

```java
    public static Node insert(Node first,int index,Node a){
        Node ret = new Node();
        ret.Next=first;//创建一个虚拟头节点
        Node p=ret;
        while((index--)!=0) p=p.Next;
        //完成节点的插入操作
        a.Next=p.Next;
        p.Next=a;
        //返回真正的链表头节点地址
        return ret.Next;//函数返回链表的头节点
    }
```

### **获取单链表的长度**

```java
    public static int GetLength(Node first){
        int n=0;
        for(Node x=first;x!=null;x=x.Next){
            ++n;
        }
        return n;
    }
```

### **打印单链表的长度**

```java
    public static void PrintLength(Node first){
        System.out.println("Length : "+GetLength(first));
    }
```

### **单链表删除指定索引的结点**

```java
    public static Node Delete(Node first,int index){
        if(index<0||index>=GetLength(first)) return first;
        else{
        Node ret=new Node();
        ret.Next=first;
        Node p=ret;
        while((index--)!=0) p=p.Next;
        //完成节点的删除操作
        p.Next=p.Next.Next;
        return ret.Next;
        }
    }
```

### **单链表实现元素查找，返回是否存在布尔值**

```java
    public static boolean Find(Node first,int key){
        for(Node x=first;x!=null;x=x.Next){
            if(x.data==key) return true;
        }
        return false;
    }
```

### **单链表删除指定索引的后续节点**

```java
    public static void RemoveAfter(Node first,int index){
        Node ret=new Node();
        ret.Next=first;
        Node p=ret;
        while((index--)!=0) p=p.Next;
        p.Next.Next=null;

    }
```

### **单链表反转**

```java
    public static Node  reverse(Node list){
        Node curr=list,pre=null;
        while(curr!=null){
            Node next=curr.Next;
            curr.Next=pre;
            pre=curr;
            curr=next;
        }
        return pre;
    }
```

### **递归地进行单链表反转**

```java
    public static Node reverseRecursively(Node head){
        if(head==null||head.Next==null) return head;//递归的终止条件，返回反转后链表的头节点
        Node reversedListHead=reverseRecursively(head.Next);
        head.Next.Next=head;//改变这两个结点之间的指向顺序
        head.Next=null;
        return  reversedListHead;//返回反转后的链表头节点
    }
```

### **检测链表中是否含有环**

```java
    public static boolean checkCircle(Node list){
        if(list==null) return false;

        Node fast=list.Next;
        Node slow=list;

        while(fast!=null&&fast.Next!=null){
            fast=fast.Next.Next;
            slow=slow.Next;

            if(slow==fast) return true;
        }
        return false;
    }
```

### **删除倒数第k个结点**

```java
    public static Node deleteLastKth(Node list,int k){
        //利用两个指针，fast和slow，它们之间差k个位置，判断如果fast.Nest=null,也就代表着slow这个位置就是倒数第k个结点
        Node fast=list;
        int i=1;
        while(fast!=null&&i<k){
            fast=fast.Next;
            ++i;
        }

        if(fast==null) return list;

        Node slow=list;
        Node prev=null;
        while(fast.Next!=null){
            fast=fast.Next;
            prev=slow;
            slow=slow.Next;
        }

        if(prev==null){
            list=list.Next;
        }else{
            prev.Next=prev.Next.Next;
        }
        return list;
    }
```

### **求中间节点**

```java
    public static Node findMiddleNode(Node list){
        if(list==null) return null;

        Node fast=list;
        Node slow=list;

        while(fast!=null&&fast.Next!=null){
            fast=fast.Next.Next;
            slow=slow.Next;
        }

        return slow;
    }
```

### **有序链表合并**

```java
    public static Node mergeTwoLists(Node l1,Node l2){
        Node soldier=new Node();
        Node p=soldier;

        while(l1!=null&&l2!=null){
            if(l1.data<l2.data){
                p.Next=l1;
                l1=l2.Next;
            }
            else{
                p.Next=l2;
                l2=l2.Next;
            }
            p=p.Next;
        }

        if(l1!=null){ p.Next=l1;}
        if(l2!=null){ p.Next=l2;}
        return soldier.Next;
    }
```



## 3.栈

- 顺序栈
- 链式栈

### 1.基于数组实现的顺序栈

- 构造函数
- 入栈操作
- 出栈操作
- 打印操作

```java
package Stack;

//基于数组实现的顺序栈
public class ArrayStack {
    private int[] items;
    private int count;//栈中的元素个数
    private int n;//栈的大小
  //初始化数组，申请一个大小为n的数组空间
public ArrayStack(int n){
    this.items=new int[n];
    this.n=n;
    this.count=0;
}

//入栈操作
public boolean push(int item){
    //数组空间不足，直接返回false，入栈失败
    if(count==n) return false;
    //将data放在下标为count的位置，并且count加一
    items[count]=item;
    ++count;
    return true;
}

//出栈操作
public int pop(){
    //栈为空，直接返回-1;
    if(count==0) return -1;
    //返回下标为count-1的数组元素，并且栈中元素个数count减一
    int tmp=items[count-1];
    --count;
    return tmp;
}
public void PrintStack(){
    for(int i=count-1;i>=0;--i){
        System.out.print(items[i]+" ");
    }
    System.out.println();
    }
}
```

### 2.基于链表的链式栈

- 入栈操作
- 出栈操作
- 打印操作

```java
package Stack;

public class LinkedListStack {
    private Node top;//栈顶（最近添加的元素）
    private int N;//元素数量
    private class Node{
        //定义了结点的嵌套类
        int data;
        Node Next;
    }
    public boolean isEmpty(){
        return top==null;
    }
    public int size(){
        return N;
    }

    public void push(int data){
        /*Node newNode=new Node();
        //判断是否为空栈
        //if(top==null) 
        newNode=top;
        top.data=data;
        top.Next=newNode;
        N++;*/
        Node newNode=top;
        top=new Node();
        top.data=data;
        top.Next=newNode;
        ++N;
    }
    public int pop(){
        //从栈顶删除元素
        if(top==null) return -1;//这里用-1表示栈中没有数据
        int data=top.data;
        top=top.Next;
        N--;
        return data;
    }
    public void PrintStack(){
        for(Node x=top;x!=null;x=x.Next){
            System.out.print(x.data+" ");
        }
        System.out.println();
    }

}
```

## 4.普通队列

- 基于数组实现的普通队列
- 基于链表实现的队列
- 基于数组实现的循环队列

### **1.基于数组实现的普通队列**

- 构造函数
- 入队操作
- 出队操作
- 打印队列中的元素

```java
package Queue;

//用数组实现队列
public class ArrayQueue {
    //数组：items，数组大小：n
    private int[] items;
    private int n=0;
    //head表示队头下标，tail表示队尾下标
    private int head=0;
    private int tail=0;

    //申请一个大小为capacity的数组
    public ArrayQueue(int capacity){
        items=new int[capacity];
        n=capacity;
    }

    //入队(一),基础版
    public boolean enqueue(int item){
        //如果tail==n，表示队列末尾已经没有空间了
        if(tail==n) return false;
        items[tail]=item;
        ++tail;
        return true;
    }

    //入队（二），改进版
    public boolean enqueue1(int item){
        //如果tail==n，表示队列末尾已经没有空间了
        if(tail==n){
            //tail==n&&head==0,表示整个队列都占满了
            if(head==0) return false;
            //数据搬移
            for(int i=head;i<tail;++i){
                items[i-head]=items[i];
            }
            //搬移完成后重新更新head和tail
            tail=tail-head;
            head=0;
        }
        items[tail]=item;
        ++tail;
        return true;
    }

    //出队
    public int dequeue(){
        //如果head==tail，表示队列为空
        if(head==tail) return -1;//这里用-1表示队列为空
        int ret=items[head];
        ++head;
        return ret;
    }

    //打印队列
    public void PrintQueue(){
        for(int i=head;i<tail;++i){
            System.out.print(items[i]+" ");
        }
        System.out.println();
    }

}
```

### 2.基于链表实现的队列

- 构造函数
- 入队操作
- 出队操作
- 打印队列中的元素

```java
package Queue;

//基于链表实现的队列
public class LinkedListQueue {

    private Node head;//指向最早添加的结点的链接
    private Node tail;//指向最近添加的结点的链接
    private int N;//队列中的元素数量
    private class Node{
        //定义了结点的嵌套类
        int data;
        Node Next;
    }
    public boolean isEmpty(){
        return head==null;
    }
    public int size(){ return N;}

    //向表尾添加元素,即入队
    public void enqueue(int data){
        Node newNode=tail;
        tail=new Node();
        tail.data=data;
        tail.Next=null;
        if(isEmpty()) head=tail;
        else newNode.Next=tail;
        ++N;
    }
    public int dequeue(){
        //从表头删除元素
        int data=head.data;
        head=head.Next;
        if(isEmpty()) tail=null;
        N--;
        return data;
    }

    //打印输出队列元素
    public void PrintQueue(){
        for(Node x=head;x!=null;x=x.Next){
            System.out.print(x.data+" ");
        }
        System.out.println();
    }
}
```

### 3.基于数组实现的循环队列

- 构造函数
- 入队操作
- 出队操作
- 打印队列中的元素

```java
package Queue;

public class CircularQueue {
    //数组items，数组大小n
    private int[] items;
    private int n=0;
    //head表示队头下标，tail表示队尾下标
    private int head=0;
    private int tail=0;

    //申请一个大小为capacity的数组
    public CircularQueue(int capacity){
        items = new int[capacity];
        n=capacity;
    }

    //入队
    public boolean enqueue(int item){
        //队列满了
        if((tail+1)%n==head) return false;
        items[tail]=item;
        tail=(tail+1)%n;//实现计数的循环
        return true;
    }

    //出队
    public int dequeue(){
        //如果head==tail，表示队列为空
        if(head==tail) return -1;//以-1表示队列为空
        int ret=items[head];
        head=(head+1)%n;
        return ret;
    }

    //打印队列
    public void PrintQueue(){
        if(n==0) return;
        for(int i=head;i%n!=tail;i=(i+1)%n){
            System.out.print(items[i]+" ");
        }
        System.out.println();
    }
}
```
全文完
