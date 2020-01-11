---
layout: post
title: 数据结构之链表
categories: [数据结构与算法]
description: 数据结构与算法学习笔记
keywords: 数据结构与算法, JavaScript, 链表, Linked List
---

主要是将自己在学习数据结构与算法中的一些比较常用的知识点归纳总结。  

[code resource](https://codepen.io/frankbar/pen/yLyjpvP?editors=0111)

**未完成**

## 1. 什么是链表。  

来看一下 LeetCode 上对链表的描述：  

 > 链表（Linked List）是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，而是在每一个节点里存到下一个节点的指针（Pointer）。

![链表](https://pic.leetcode-cn.com/67c0f9acaaaa44685a22fd85eaaba409341f874b99a5c953ff8efbc8d5110e02-image.png)  
 > 由于不必须按顺序存储，链表在插入的时候可以达到 O(1)O(1) 的复杂度，比另一种线性表 —— 顺序表快得多，但是查找一个节点或者访问特定编号的节点则需要 O(n)O(n) 的时间，而顺序表相应的时间复杂度分别是 O(log\ n)O(log n) 和 O(1)O(1)。

 > 使用链表结构可以克服数组链表需要预先知道数据大小的缺点，链表结构可以充分利用计算机内存空间，实现灵活的内存动态管理。但是链表失去了数组随机读取的优点，同时链表由于增加了结点的指针域，空间开销比较大。

 > 在计算机科学中，链表作为一种基础的数据结构可以用来生成其它类型的数据结构。链表通常由一连串节点组成，每个节点包含任意的实例数据（data fields）和一或两个用来指向上一个/或下一个节点的位置的链接（links）。链表最明显的好处就是，常规数组排列关联项目的方式可能不同于这些数据项目在记忆体或磁盘上顺序，数据的访问往往要在不同的排列顺序中转换。而链表是一种自我指示数据类型，因为它包含指向另一个相同类型的数据的指针（链接）。

 > 链表允许插入和移除表上任意位置上的节点，但是不允许随机存取。链表有很多种不同的类型：单向链表，双向链表以及循环链表。

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

``` javascript
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
        this.head = new LinkedNode("")
    }
	head: LinkedNode 
}
```

## 4. 链表的操作
链表也应该支持查询 / 插入 / 删除 等操作，先来实现链表的查询：  

### 4.1 链表的查询 





``` javascript
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
    find(nodeValue:any) {
        let curNode:LinkedNode = this.head

        while( curNode.value != nodeValue ) {
            if(!curNode.next) {
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

``` javascript
    /**
     * @author Frank Wang
     * @method
     * @name insert
     * @description 向链表中插入节点
     * @param nodeValue 已经存在的 node 节点的 value
     * @param value 向链表中插入的节点的 value
     * @return {LinkedNode}
     * @example 创建例子。
     * @public
     */
    insert(nodeValue:any,value:any) {
        let newNode:LinkedNode = new LinkedNode(value)
        let curNode:LinkedNode = this.find(nodeValue)
        newNode.next = curNode.next
        curNode.next = newNode
        return newNode
    }
```

插入的方法，接受两个参数，一个是已存在的 node 节点的 value，一个是需要插在 node 节点后的节点的 value。函数体首先是根据传入的 value 创建一个 node 实例，以及找到需要插入的node节点。然后将当前节点的 next 赋值给新的节点实例，在将新节点，赋值给当前节点的 next 值。尤其注意这两步的操作，是不能调换的，可以想像为什么。

### 4.3 展示链表

插入链表之后我们肯定是想知道链表的当前情况，或者是我们在某个时刻想要查看链表的情况，那我们就需要一个方法来展示链表：

``` javascript
    /**
     * @author Frank Wang
     * @method
     * @name console
     * @description 将链表所有的值在控制台打印出来
     * @param 
     * @return {LinkedNode}
     * @example 创建例子。
     * @public
     */
    console() {
        let currNode:LinkedNode = this.head;
        while ( !currNode.next ){
            console.log( currNode.next.value );
            currNode = currNode.next;
        }
        return this.head
    }
```

同样的我们还会遇到删除某一节点的需求：

### 4.4 删除链表中某一节点  

在解决这个需求之前，我们可以先来分析一下，如何删除一个链表中的节点，就是将此节点的上一节点的 next 指向当前节点的 next。

所以如果我们要删除某一节点，那么我们就需要知道当前节点的上一个节点

```javascript
LinkedList.prototype.remove = function ( value ) {
    let curNode = this.head
    let prevNode 
    while (curNode) {
        if(curNode.value === value) {
            break;
        }
        prevNode = curNode
        curNode = curNode.next
    }
    if ( curNode === null) {
        return false
    }
    if (curNode.value == this.head.value) {
        this.head = this.head.next
        return
    }
    prevNode.next = curNode.next
}
```

解释一下上面的逻辑：首先是定义一个当前节点，方便进行遍历。还有一个上一节点 `prevNode` 默认为 `undefined`，然后是对链表进行遍历，每一次遍历都会修改当前的节点 `curNode` 以及 `prevNode` ,直到当前节点的值是要删除的节点的值，那么退出循环，同时将上一节点的 `next`，改为当前节点 `curNode` 的 `next`，亦或者写做 `prevNode.next = prevNode.next.next`。其他的判断就是当是尾节点时返回 `false`，当是 `head` 节点时，就使用 `head.next`替换 `head` 节点；

除了删除我们还会遇到向链表中插入节点的情况： 

### 