---
layout: post
title: LeetCode No.203 移除链表元素
categories: [LeetCode]
description: 移除链表元素
keywords: LeetCode, JavaScript,Remove Linked List Elements,linked
---

# No.203 移除链表元素

## 题目  

删除链表中等于给定值 val 的所有节点。  

测试用例:  
  - 输入: 1->2->6->3->4->5->6, val = 6 ;输出: 1->2->3->4->5 
  - 输入: 1->1,val = 1 ; 输出: null


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
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function(head, val) {

};

```

## 思路

想到的是进行遍历判断，如果节点的 `val` 值与方法的第二个参数相同，就将此节点的 `next` 的值赋值给上一节点的 `next` 属性，从而替换当前节点，达到目的。但是在链表中，我们没有办法获取到当前节点的上一节点。问题就陷入了死胡同，咋整呢？  

我们可以为传入的链表增加一个链表头。然后判断当前节点的 `next` 属性的 `val` 是否与传入的相同。

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
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function(head, val) {
    if(head === null) return null

    let tmp = new ListNode(-1)
    tmp.next = head
    let iteratorTmp = tmp

    while(iteratorTmp.next !== null) {
        if(iteratorTmp.next.val == val) {
            iteratorTmp.next = iteratorTmp.next.next
        } else {
            iteratorTmp = iteratorTmp.next;
        }
        
    }
    return tmp.next
};

```

## 解析  

如上述说的那样，我们增加了一个指向新链表的指针。通过遍历这个指针，达到改变链表的目的。当当前节点的 `next` 的 `val` 值与第二个参数相同时，就执行赋值操作。

## 进阶解答  

``` javascript

var removeElements = function(head, val) {
    if (!head) {
        return null;
    }
    head.next = removeElements(head.next, val);
    
    if (head.val === val) {
        return head.next;
    } else {
        return head;
    }
};

```

很明显的，递归解法。这有点儿像我们刚开始想的那样，遍历链表，然后使用节点的 `next` 属性替换当前节点。

## 想法  

做了有几道关于链表的题目，发现链表的一些题目比较适合用递归的方式来解决。

## 其他链表类型题目
  - [删除链表中的某个节点](http://slowboat.top/2019/11/21/delete-node-in-linked-list/)
  - [反转链表](http://slowboat.top/2019/12/01/reverse-linkedlist/)
  - [合并两个有序链表](http://slowboat.top/2019/12/02/Merge-Two-Sorted-Lists/)
  - [相交链表](http://slowboat.top/2019/12/03/Intersection-Two-Linked-Lists/)