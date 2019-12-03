---
layout: post
title: LeetCode No.21 合并两个有序链表
categories: [LeetCode]
description: 合并有序链表
keywords: LeetCode, JavaScript
---

# No.21 合并两个有序链表

## 题目  

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。  

测试用例:
  - 输入： 1->2->4, 1->3->4 -> 输出：1->1->2->3->4->4 


``` javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {

};
```

## 思路

既然要合并两个有序列表，而链表又没有合并方法，所以想到的是将链表每个节点转换为数组的 item ，再对数组的每个 item 进行排序，改变 每个 item 的 `next` 即可做到。

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
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    if(!l1&&!l2) return null
    if(l1==null && l2!==null) return l2
    if(l2==null && l1!==null) return l1

    function convert(listnode,arr) {
        while(listnode) {
            arr.push(listnode)
            listnode = listnode.next
        }
        return arr
    }
    let arr1 = convert(l1,[])
    let arr2 = convert(l2,[])
    let result = [...arr1,...arr2].sort((a,b)=>a.val-b.val)
 
    for(let i = 0,len=result.length;i<len;i++) {
        if(i === len-1) {
            result[i].next = null
        } else {
            result[i].next = result[i+1]
        }
    }
    return result[0] 
};
```

## 解析  

主要想法是将链表转为数组，利用数组的合并以及排序功能得到合并以后的有序链表。

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
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
  if (l1 === undefined || l1 === null) return l2;
  if (l2 === undefined || l2 === null) return l1;

  const current = new ListNode('');
  if (l1.val >= l2.val) {
    current.val = l2.val;
    current.next = mergeTwoLists(l1, l2.next)
  } else {
    current.val = l1.val;
    current.next = mergeTwoLists(l1.next, l2);
  }
  return current;
};
```

完全是对链表的操作，没有了数组占据的空间复杂度。通过逐个比较两个有序链表的节点的 `val` 值，进行递归。

## 想法  

第一遍的做法明显是很笨拙的做法，占用了太多的内存空间。看了别人的解答，运用了递归，通过改变一个新节点的 `next` 属性的指向，从而达到目的。同样的，也是对链表节点知识的考察。

## 其他链表类型题目
  - [删除链表中的某个节点](http://slowboat.top/2019/11/21/delete-node-in-linked-list/)
  - [反转链表](http://slowboat.top/2019/12/01/reverse-linkedlist/)