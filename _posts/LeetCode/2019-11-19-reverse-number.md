---
layout: post
title: LeetCode No.7 整数反转
categories: [LeetCode]
description: 整数反转
keywords: LeetCode, JavaScript
---

# 7.整数反转

## 题目  

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。  
**注意** 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。  
测试用例:  
	- reverse(123) -> 321
	- reverse(-123) -> -321
	- reverse(120) -> 21  

``` javascript
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    return x;
};
```

## 思路

首先想到的就是 `Array.prototype.reverse()` 方法,但是 `reverse()` 方法是数组的方法,所以我们需要先将 number 取绝对值后使用此方法进行翻转,而后在使用`Array.prototype.join()` 将数组拼接起来,在组成number 类型,反出去.同时判断结果是不是在上文要求的范围内.  

## 第一遍解答

``` javascript
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    if(typeof x !== 'number') {
        throw new TypeError('请输入数字')
    }
    let overflow = function(num) {
        if(num >= Math.pow(2, 31) - 1 || num <= -Math.pow(2, 31)) {
            return 0;
        }
        return num;
    }

    let reversePositive = function(num) {
        return Array.from(String(num)).reverse().join("")-0
    }

    let result = 0
    if(x < 0) {
        result =  -1 * reversePositive(Math.abs(x))
    } else {
        result = reversePositive(x)
    }
    return overflow(result)
};
```

## 解析  

按照上述思路,将 number 取绝对值后传入反转函数中,最后在判断是否超出范围.  
此答案用时 `92ms`.超过`69.28%`的答案.  
让我们来看一下执行为 `60ms` 的答案

## 进阶解答  

``` javascript
/**
 * @param {number} x
 * @return {number}
 * 答案来自 leetcode
 */
var reverse = function(x) {
	let a = ''
	if ( x < 0 ) a = '-'
	let data = Math.abs(x).toString().split('')
	if ( data.length < 2 ) return x
	if ( data.length >= 32 ) return 0

	a = a + Number(data.reverse().join(''))
  	if ( a > (Math.pow(2, 31) - 1) || a < Math.pow(-2, 31) ) {
    	return 0
  	} else{
    	return a
  	}
};
```

主要是将负数的处理放在了开始,并包含了隐式转换.同时也通过 `if`语句判断,做到及时返回.  

## 想法  

简单的这种题目,可以尽早的做判断从而早`return` 结束语句.从而达到减少执行时间的目的.  
