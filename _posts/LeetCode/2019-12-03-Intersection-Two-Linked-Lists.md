---
layout: post
title: LeetCode No.160 相交链表
categories: [LeetCode]
description: 相交链表
keywords: LeetCode, JavaScript, Intersection
---

# No.160 相交链表

## 题目

编写一个程序，找到两个单链表相交的起始节点。

![相交链表示例](https://raw.githubusercontent.com/FrankWang117/images/master/AnaYhX.png)

上图两个链表在节点 c1 开始相交。

测试用例:

- 输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
  输出：Reference of the node with value = 8

注意：

- 如果两个链表没有交点，返回 null;
- 在返回结果后，两个链表仍须保持原有的结构;
- 可假定整个链表结构中没有循环;
- 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function (headA, headB) {};
```

## 思路

寻找两个链表的相交节点可以通过节点的 `next` 属性进行遍历，这在之前的题目中也运用过。虽然时间复杂度达到了 O(mn),

## 第一遍解答

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function (headA, headB) {
  while (headA) {
    let tmp = headB;
    while (tmp) {
      if (headA === tmp) return headA;
      tmp = tmp.next;
    }
    headA = headA.next;
  }
};
```

## 解析

不能改变原有链表的结构，所以有一个中间变量 `tmp`。通过“暴力”的两个链表迭代，将相同节点寻找出来。

## 进阶解答

```javascript
// 解答一
var getIntersectionNode1 = function (headA, headB) {
  while (headA) {
    headA.sep = 1;
    headA = headA.next;
  }
  while (headB) {
    if (headB.sep) return headB;
    headB = headB.next;
  }
};
/* 解答二*/
var getIntersectionNode2 = function (headA, headB) {
  var pA = headA;
  var pB = headB;
  while (pA !== pB) {
    pB = pB ? pB.next : headA;
    pA = pA ? pA.next : headB;
  }
  return pA;
};
```

解法一比较有意思的点在于通过遍历一个链表 A，为链表中的每一项打上类似 `tag` 的东西 `sep` ,然后遍历另一个链表 B，寻找节点中有无打上 `sep` 标签的节点，由于节点的 `next` 是引用指针，所以，如果 B 中存在打上 `sep` 标签的节点，那么两个链表就有相交节点。

解法二的解释是：

> 双指针法。初始化两个指针 pA 和 pB 分别指向 headA 和 headB，每次 pA 和 pB 各走一步，当 pA 触底后变轨到 headB，同理，当 pB 触底后变轨到 headA。这样就只需遍历（A 的非公共部分+B 的非公共部分+AB 的公共部分）。[详细解释](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/solution/javascriptxiang-jiao-lian-biao-tu-jie-shuang-zhi-z/)

## 想法

不得不说这个双指针法真的是妙得很，当然标记方法也是很不错，虽然时间复杂度比双指针多，

## 其他链表类型题目

- [删除链表中的某个节点](http://slowboat.top/2019/11/21/delete-node-in-linked-list/)
- [反转链表](http://slowboat.top/2019/12/01/reverse-linkedlist/)
- [合并两个有序链表](http://slowboat.top/2019/12/02/Merge-Two-Sorted-Lists/)
