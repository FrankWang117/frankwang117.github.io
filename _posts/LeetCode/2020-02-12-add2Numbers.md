---
layout: post
title: LeetCode No.2 两数相加
categories: [LeetCode,ListNode]
description: 两数相加
keywords: LeetCode, JavaScript, Add Two Numbers
---

# No.2 两数相加

## 题目  

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
示例：
```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

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
var addTwoNumbers = function(l1, l2) {

};
```

## 思路
想当然的是对两个链表进行遍历，并将链表当前的 `val` 进行相加，对相加的结果进行处理，并放入遍历外层的 result 中。

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
var addTwoNumbers = function(l1, l2) {
    let result = new ListNode(0);
    let nextResult = result;
    let overTen = 0
    let val = 0
    while(l1 || l2 || overTen === 1) {
        let l1Val = l1? l1.val:0
        let l2Val = l2? l2.val:0;

        let add = l1Val + l2Val+overTen;

        if(add > 9) {
            overTen = 1
            val = add - 10
        } else {
            overTen = 0;
            val = add
        }
        nextResult.next = new ListNode(val)
        nextResult = nextResult.next

        l1 = l1? l1.next:null
        l2 = l2?l2.next:null
    }
    return result.next
};
```

## 解析  

按照上述思路,在函数内部定义了四个变量，一个是当前函数的初始化结果，是个链表，第二个是用于循环内获得循环的结果，也是个链表。第三个是判断上一循环两数相加是否大于10。第四个是当前循环两数相加的和。  
循环整体来看，是内部定义了两个变量，`l1Val` 以及 `l2Val`，并根据这两个以及 `overTen` 变量的和判断 `val` 的值，并求出 `nextResult` 的值。最后返回 `result.next` 链表。

## 进阶解答  

``` javascript
var addTwoNumbers = function(l1, l2) {
    let result = new ListNode(0);
    let curr = result;
    let carry = 0 // 进位
    let val = 0 // 传给当前层级的值
    
    while(l1 != null || l2 != null) {

        let x = (l1 != null) ? l1.val : 0;
        let y = (l2 != null) ? l2.val : 0;
        
        val = (x + y + carry) % 10;
        carry = Math.floor((x + y + carry) / 10);
       
        curr.next = new ListNode(val) 
        curr = curr.next
        
        if(l1 != null) l1 = l1.next
        if(l2 != null) l2 = l2.next        
    
    }
    
    if(carry) {
       curr.next = new ListNode(carry)
    }
    
    return result.next
};
```

>伪代码如下[^1]：  
> - 将当前结点初始化为返回列表的哑结点.  
> - 将进位 carry 初始化为 0。  
> - 遍历列表 l1 和 l2 直至到达它们的尾端。
>   - 将 x 设为结点 l1 的值。如果已经到达 l1 的末尾，则将其值设置为 0。
>   - 将 y 设为结点 l2 的值。如果已经到达 l2 的末尾，则将其值设置为 0。  
>   - 设定 sum = x + y + carry。  
>   - 更新进位的值，carry = sum / 10。  
>   - 创建一个数值为 (sum mod 10) 的新结点，并将其设置为当前结点的下一个结点，然后将当前结点前进到下一个结点。  
>   - 同时，将 l1 和 l2 前进到下一个结点。  
> - 检查 carry = 1 是否成立，如果成立，则向返回列表追加一个含有数字 11 的新结点。  
> - 返回哑结点的下一个结点。  

## 想法  

主要是考虑的方面需要考虑到一些特殊的测试用例。  

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers


[^1]: [来源](https://leetcode-cn.com/problems/add-two-numbers/solution/liang-shu-xiang-jia-by-leetcode/)  