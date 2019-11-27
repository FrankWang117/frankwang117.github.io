---
layout: post
title: LeetCode No.237 删除链表中的节点
categories: [LeetCode]
description: 删除链表中的节点
keywords: LeetCode, JavaScript, 链表, LinkedList
---


# No.237 删除链表中的节点

## 题目  

请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点，你将只被给定要求被删除的节点。  
测试用例:  
  - 输入: head = [4,5,1,9], node = 5;输出: [4,1,9]
  - 输入: head = [4,5,1,9], node = 1;输出: [4,5,9]

说明:
  - 链表至少包含两个节点。
  - 链表 中所有节点的值都是唯一的。
  - 给定的节点为非末尾节点并且一定是链表中的一个有效节点。
  - 不要从你的函数中返回任何结果。


``` javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} node
 * @return {void} Do not return anything, modify node in-place instead.
 */
var deleteNode = function(node) {
    
};
```

## 思路

看了好几遍这个题目，以及提供的测试用例，没太整明白要怎么做。毕竟给出的 `deleteNode` 方法接受的是一个参数，类型为 `ListNode` ，这个类型看起来像是链表的构造函数的简化版。但是要完成 `deleteNode` 这个方法真的就不给要从哪个链表中删除此 `node` 节点么？看到这里就会很是迷惑。  

既然有了迷惑，那就慢慢解决，先来解决下什么是链表。  
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

``` javascript
// 节点
function ListNode(value) {
    this.value = value
    this.next = null
}
// 链表
class LinkedList {
    constructor(value) {
        this.head = new ListNode(value)
    }
}
```

链表也应该支持查询 / 插入 / 删除 等操作，先来实现链表的查询：  

``` javascript
LinkedList.prototype.find = function ( value ) {
    let curNode = this.head;
    while ( curNode.value != value ){
        curNode = curNode.next;
    }
    return curNode;
}
```
主要是一个内部的循环，遍历整个链表，判断当前节点的 `value` 值是否与要查找的值相同。  
既然有了查找，那我们删除某一节点就有思路了：

``` javascript
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
解释一下上面的逻辑：首先是定义一个当前节点，方便进行遍历。还有一个上一节点 `prevNode` 默认为 `undefined`，然后是对链表进行遍历，每一次遍历都会修改当前的节点 `curNode` 以及 `prevNode` ,直到当前节点的值是要删除的节点的值，那么退出循环，同时将上一节点的 `next`，改为当前节点 `curNode` 的 `next`，亦或者写做 `prevNode.next = prevNode.next.next`。其他的判断就是当是尾节点时返回 `false`，当是 `head` 节点时，就使用 `head.next`替换 `head` 节点。  
这是单向链表的两个基本的方法，后期应该会把链表写成一篇单独的学习笔记，总结归纳。

## 第一遍解答

``` javascript
// 。。。
```

## 解析  

说实话第一遍解答真没解答出来。

## 进阶解答  

``` javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} node
 * @return {void} Do not return anything, modify node in-place instead.
 */
var deleteNode = function(node) {
    node.val = node.next.val
    node.next = node.next.next
};
```

看了这个答案之后，真的是妙啊。妙就妙在直接将当前节点进行了替换，因为这里面传入的不再是我们上文写的方法中的形参 `value` 了，而是一个节点，所以这里只需要将当前节点的下一节点的 `val` / `next` 替换成当前节点，就可以了。

## 想法  
还是对数据结构中链表的不太熟悉，从这篇开始，会以不同类别的形式，集中的做题，并分析。  
个人觉着这道题目对审题以及知识的灵活运用都是很好的一次检验。从而也暴露出自己目前对数据结构掌握的还是不够熟练的问题。  
妙，这题是真的妙。