---
layout: post
title: 数据结构之链表
categories: [数据结构与算法]
description: 数据结构与算法学习笔记
keywords: 数据结构与算法, JavaScript, 链表, Linked List
---

线性表的一大结构：链式存储结构的一些知识点总结归纳。并使用 JavaScript 来实现简单的单链表。循环链表以及双向链表是在单链表的基础上扩展。  

[code resource](https://codepen.io/frankbar/pen/yLyjpvP?editors=0111)


## 1. 什么是链表。  

来看一下 LeetCode 上对链表的描述：  

 > 链表（Linked List）是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，
 而是在每一个节点里存到下一个节点的指针（Pointer）。

![链表](https://pic.leetcode-cn.com/67c0f9acaaaa44685a22fd85eaaba409341f874b99a5c953ff8efbc8d5110e02-image.png)  
 > 由于不必须按顺序存储，链表在插入的时候可以达到 O(1)O(1) 的复杂度，比另一种线性表 —— 顺序表
 快得多，但是查找一个节点或者访问特定编号的节点则需要 O(n)O(n) 的时间，而顺序表相应的时间复杂
 度分别是 O(log\ n)O(log n) 和 O(1)O(1)。

 > 使用链表结构可以克服数组链表需要预先知道数据大小的缺点，链表结构可以充分利用计算机内存空间，
 实现灵活的内存动态管理。但是链表失去了数组随机读取的优点，同时链表由于增加了结点的指针域，
 空间开销比较大。

 > 在计算机科学中，链表作为一种基础的数据结构可以用来生成其它类型的数据结构。链表通常由一连串节
 点组成，每个节点包含任意的实例数据（data fields）和一或两个用来指向上一个/或下一个节点的位置
 的链接（links）。链表最明显的好处就是，常规数组排列关联项目的方式可能不同于这些数据项目在记忆
 体或磁盘上顺序，数据的访问往往要在不同的排列顺序中转换。而链表是一种自我指示数据类型，因为它包
 含指向另一个相同类型的数据的指针（链接）。

 > 链表允许插入和移除表上任意位置上的节点，但是不允许随机存取。链表有很多种不同的类型：单向链表，
 双向链表以及循环链表。

 > 链表通常可以衍生出循环链表，静态链表，双链表等。对于链表使用，需要注意头结点的使用。  

以上大概就是对链表的一个说明，那用代码来展示一个链表的话是什么样子呢?  
我们先从一个简单的单向链表尝试做一下：

## 2. 链表的表现形式

``` javascript
const linkedList = {
    head: {
        value: 0,
        next: {
            value: 1,
            next: {
                value: 2,
                next: null
            }
        }
    }
};
```

与图片上展示的链表结构相同：每个节点都有一个 `value` 属性，展示当前节点的值，以及一个指向下一个节点（保存指向下一个节点的指针）的属性 `next`。这构成了一个节点。多个节点构成了链表。同时也可以看到在链表的头部，使用 `head` 来表示，在链表的末端节点 `next` 指向为 `null`。  

既然知道了链表的展现形式，那就尝试使用代码将链表生成：  

## 3. 链表的生成  

``` typescript
// 节点
class LinkedNode {
    value: any 
    next: any
    constructor(nodeValue) {
        this.value = nodeValue;
        this.next = null
    }
}
// 链表
class LinkedList {
    constructor() {
        // 生成链表头部，链表头部的数据域中可以不存储任何信息
        // 也可以存储链表长度等信息
        // 这里为了区分，使头节点的数据为 'head'
        this.head = new LinkedNode("head")
    }
    private head: LinkedNode
}
```

## 4. 链表的操作
链表也应该支持查询 / 插入 / 删除 等操作，先来实现链表的查询：  

### 4.1 链表的查询 

查找一个元素是否在链表中是链表操作中比较常用的操作：

``` typescript
    /**
     * @author Frank Wang
     * @method
     * @name find
     * @description 查询链表中是否有符合条件的节点
     * @param nodeValue {any} 所要查询的 node 节点的 value 值
     * @return {LinkedNode}
     * @example 创建例子。
     * @public
     */
    find(nodeValue: any): LinkedNode {
        let curNode: LinkedNode = this.head

        while (curNode.value != nodeValue) {
            if (!curNode.next) {
                // 如果到链表尾部还没有循环到符合条件的节点
                // 则返回 value 值为 error 对象的节点
                return new LinkedNode(new Error("无此节点"))
            }
            curNode = curNode.next
        }
        return curNode
    }
```
主要是一个内部的循环，遍历整个链表，判断当前节点的 `value` 值是否与要查找的值相同。  
既然有了查找，那我们向链表中插入某一节点就有思路了：

### 4.2 向链表中插入节点

向传入的节点的后方插入节点

``` typescript
    /**
     * @author Frank Wang
     * @method
     * @name insert
     * @description 向链表中插入节点,第二个参数为空的话，向尾部插入
     * @param value 向链表中插入的节点的 value
     * @param nodeValue 已经存在的 node 节点的 value
     * @return {LinkedNode}
     * @example 创建例子。
     * @public
     */
    insert(value: any, nodeValue: any = null):LinkedNode {
        let newNode: LinkedNode = new LinkedNode(value)
        let curNode: LinkedNode

        if (!nodeValue) {
            // append 的方法
            // 查找倒数第二个节点
            curNode = this.head
            while (curNode.next !== null) {
                curNode = curNode.next
            }
            curNode.next = newNode
            newNode.next = null
        } else {
            curNode = this.find(nodeValue)
            newNode.next = curNode.next
            curNode.next = newNode
        }

        return this.head
    }
```

插入的方法，接受两个参数：一个是新的 node 节点的 value，另一个是可选的，已经存在的 node 节点
的 value，如果只传入一个参数，则往链表尾部插入。函数体首先是根据传入的 value 创建一个 node 
实例，以及找到需要插入的node节点。然后将当前节点的 next 赋值给新的节点实例，在将新节点，
赋值给当前节点的 next 值。尤其注意这两步的操作，是不能调换的，可以想像为什么。

### 4.3 展示链表

插入链表之后我们肯定是想知道链表的当前情况，或者是我们在某个时刻想要查看链表的情况，那我们就需要
一个方法来展示链表：

``` typescript
    /**
     * @author Frank Wang
     * @method
     * @name console
     * @description 将链表所有的值在控制台打印出来
     * @param 
     * @return {void}
     * @example 创建例子。
     * @public
     */
    console(): void {
        let currNode: LinkedNode = this.head;
        while (currNode.next) {
            console.log(currNode.next.value);
            currNode = currNode.next;
        }
    }
```
这个是打印出除了头节点的其他所有节点的 value 值。如果只是单纯的获取节点对象，那么可以使用：  

``` typescript
    show() {
        return this.head
    }
```

同样的我们还会遇到删除某一节点的需求：

### 4.4 删除链表中某一节点  

在解决这个需求之前，我们可以先来分析一下，如何删除一个链表中的节点，就是将此节点的上一节点的
 next 指向当前节点的 next。

所以如果我们要删除某一节点，那么我们就需要知道当前节点的上一个节点

``` typescript
    private findPrev(value: any) {
        let curNode: LinkedNode = this.head
        while (curNode.next !== null && curNode.next.value !== value) {
            curNode = curNode.next
        }
        return curNode
    }
    /**
     * @author Frank Wang
     * @method
     * @name delete
     * @description 删除链表中的节点
     * @param {nodeValue} 需要删除的节点的 value
     * @return {LinkedNode}
     * @example 创建例子。
     * @public
     */
    delete(nodeValue: any) {
        const err = "[object Error]"
        const showType = Object.prototype.toString
        let curNode = this.find(nodeValue)
        if (showType.call(curNode) === err) {
            return curNode
        }
        const prevNode = this.findPrev(nodeValue);
        if (prevNode.next !== null) {
            prevNode.next = prevNode.next.next;
        }
        return this.head;

    }
```

解释一下上面的逻辑：首先是进行判断，对错误条件尽早返回。然后是使用内部方法 `findPrev` ，查找
所要删除节点的上一节点： `prevNode` ，
然后就是简单的 `next` 指针的改变。 

### 4.5 单向链表的总结  
单链表结构和顺序存储结构做对比：  
  - 存储分配方式
    - 顺序存储结构用一段连续的存储单元一次存储线性表的数据元素。  
    - 单链表采用链式存储结构，用一组任意的存储单元存放线性表的元素
  - 时间性能
    - 查找
      - 顺序存储结构O(1)
      - 单链表O(n)
    - 插入和删除
      - 顺序存储结构需要平均移动表长一半的元素，时间为 O(n)
      - 单链表再找出某位置的指针后，插入和删除时间仅为O(1)
  - 空间性能
    - 顺序存储结构需要预分配存储空间，分大了，浪费。分小了，易发生上溢。
    - 单链表不需要分配存储空间，只要有就可以分配，元素个数也不受限制。

## 5. 循环链表
将单链表中终端节点的指针端由空指针改为指向头节点，就使整个单链表形成了一个环，这种头尾相接的单链表称为单循环链表，简称循环链表。
## 6. 双向链表
双向链表是在单链表的每个节点中，再设置一个指向其前驱节点的指针域。  
所以在双向链表中的节点都有两个指针域，一个指向直接后继，另一个指向直接前驱。  
### 6.1 插入
如果需要将 s 节点，插入到 p 节点和 p.next 节点之中，那这个插入操作的顺序是啥？

  - 把 p 赋值给 s 的前驱
  - 把 p.next 赋值给 s 的后继节点
  - 把 s 赋值给 p.next 的前驱
  - 把 s 赋值给 p 的后继


## 5. 总结
<table>
    <thead>
        <th colspan="5" style="text-align:center">线性表</th>
    </thead>
    <tbody>
        <tr>
            <td>
            顺序存储结构
            <td>
            <td colspan="4" style="text-align:center">链式存储结构</td>
        </tr>
        <tr>
            <td>
            <td>
            <td>单链表</td>
            <td>静态链表</td>
            <td>循环链表</td>
            <td>双向链表</td>
        </tr>
    </tbody>
</table>
这是链表属于线性表的两大结构之一：链式存储结构。
