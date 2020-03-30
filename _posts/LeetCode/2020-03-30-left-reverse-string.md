---
layout: post
title: LeetCode 面试题 No.58 II 左旋转字符串
categories: [LeetCode]
description: 面试题 No.58 II 左旋转字符串
keywords: LeetCode, JavaScript, 左旋转字符串
---

# 面试题 No.58 II 左旋转字符串  
一道非常简单的题目,但是同时又有多种不一样的解法.  

## 题目  

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。  

示例：
```
输入：s = "abcdefg", k = 2
输出："cdefgab"
```

``` javascript
/**
 * @param {string} s
 * @param {number} n
 * @return {string}
 */
var reverseLeftWords = function(s, n) {
        
};
```

## 思路
首先想到的是将字符串转换为数组,利用数组的方法来完成题目,也就是:  


## 第一遍解答

``` javascript
/**
 * @param {string} s
 * @param {number} n
 * @return {string}
 */
var reverseLeftWords = function(s, n) {
    let arr = s.split(''),
        frontStr = arr.splice(0,n);
    
    return [...arr,...frontStr].join('');
        
};
```

## 解析  

可以看出来,可还是挺繁琐的,相对于这么简单的题目来说.所以我们再次思考,使用纯 string 的方法来完成题目:

```javascript
var reverseLeftWords = function(s, n) {
    return (s + s).substr(n,s.length);
        
};
```
```javascript
var reverseLeftWords = function(s, n) {
    return s.slice(n).concat(s.slice(0,n))    
};
```
## 想法  
主要是字符串方法的考察:
 - substr
 - slice
 - concat



>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/solution/zuo-xuan-zhuan-zi-fu-chuan-by-natural-selection-2/

