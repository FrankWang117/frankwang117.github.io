---
layout: post
title: LeetCode No.206 反转链表
categories: [LeetCode]
description: 反转一个单链表。
keywords: LeetCode, JavaScript, 链表, LinkedList
---


# No.205 反转链表  

[No.237 删除链表中的节点](http://slowboat.top/2019/11/21/delete-node-in-linked-list/)  

## 题目  

反转一个单链表。

测试用例:  
  - 输入: 1->2->3->4->5->null;输出: 5->4->3->2->1->null

进阶:
  - 你可以迭代或递归地反转链表。


``` javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {

};
```

## 思路
在上一节关于链表的题目中，知道了链表的一些知识。包括链表的结构，以及链表中 `val` 以及 `next` 的值的意义。这道题是对单链表进行反转。我们知道对数组进行反转很容易，可以使用 `reverse()` 方法，也可以自己使用循环封装自己的 `reverse()` 方法。但是，很明显，单链表的反转和此大不相同。但同时我们也提到了链表中的节点的 `next` 属性存储的是下一个节点的指针。这句话就是解题的关键。我们可以将链表拆分为数组，通过遍历数组，修改数组中每一项节点的 `next` 项为需要的指针，那么我们就能达到反转节点的目的：
 


## 第一遍解答

``` javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    if(head === null || head.next === null) return head;
    let res = [];
    // 此链表没有 length 属性
    while(head) {
        res.unshift(head)
        head = head.next
    }
    for(let i = 0,len = res.length;i < len;i++) {
        if(i === len - 1) {
            res[i].next = null
        } else {
            res[i].next = res[i + 1]
        }
    }
    return res;
};
```

## 解析  

第一个判断就是及早 `return` 一些其他情况。这里是将非链表以及只有一个节点的链表原样 `return`;  
再往下就是创建了一个内部的数组，用来存储分割后的链表。`unshift` 呢，就是在这一步将链表的每个节点进行反转。分割节点利用的是 [No.237](http://slowboat.top/2019/11/21/delete-node-in-linked-list/) 删除一个节点的思路。  
对链表节点数组进行遍历时，条件就是是不是最后一个节点，是的话，最后一个节点的 `next` 值为 `null`，不是的话，`next` 的值则是下一个节点。

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
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
  if (!head || !head.next) {
    return head;
  }

  const next = head.next;
  const list = reverseList(next);

  next.next = head;
  head.next = null;

  return list;
};
```

提前返回意思差不多。后面是利用了递归的写法。可能比较难以理解的是最后两行：  

``` javascript
    // ...
    next.next = head
    head.next = null
```

但是我们要把上面的一行也加入进来，也就是：  

``` javascript
    // ...
    const next = head.next; /* line 1*/
    // ...
    next.next = head; /* line 2*/
    head.next = null; /* line 3*/
```  
就是说在上述标示中的 `line1` 与 `line2` 中，`head.next` 与 `head.next.neaxt` 变成了循环引用，需要在此将循环引用给释放掉。  

## 想法  
对链表的结构以及具体的意义掌握的不够熟悉，同时对递归的运用也是不够熟练。容易造成比较大的问题。下一步要做的是可以从链表类型的题目中不断学习链表的知识，或者是专门学习链表的知识。