---
layout: post
title: LeetCode No.9 回文数
categories: [LeetCode]
description: 回文数
keywords: LeetCode, JavaScript,palindrome,回文数
---

# No.9 回文数

## 题目  

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。  
测试用例:  
  - isPalindrome(121) -> true
  - isPalindrome(-121) -> false
  - isPalindrome(10) -> false

``` javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    return true;
};
```

## 思路

首先想到的就是 `Array.prototype.reverse()` 方法,但是 `reverse()` 方法是数组的方法,所以我们需要先将 number 取值后使用此方法进行翻转,而后在使用`Array.prototype.join()` 将数组拼接起来,在与上一步转换成 string 类型的进行比较。

## 第一遍解答

``` javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if(x<0 || x%10 === 0 && x!== 0) return false;
    if(x<10) return true;
    let strX = x.toString()
    let reverseStrX = Array.from(strX).reverse().join("")
    return strX === reverseStrX
};
```

## 解析  

按照上述思路,将 number 取绝对值后传入反转函数中,最后在判断是否超出范围.  
此答案用时 `272ms`.超过`53.24%`的答案.  
让我们来看一下执行为 `172ms` 的答案

## 进阶解答  

``` javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    var y = 0, i=0,a = x;
    while(a){
        y += a%10;
        a = parseInt(a/10);
        if(a!=0) y*=10;
        i++
    }
    if(x<0){
        y = Math.abs(y) + '-'
    }
    return y==x
};
```

对数字进行循环，将反转后的数字与当前数字比较。

## 想法  

 