---
layout: post
title: LeetCode No.1299 将每个元素替换为右侧最大元素
categories: [LeetCode]
description: No.1299 将每个元素替换为右侧最大元素
keywords: LeetCode, JavaScript, 将每个元素替换为右侧最大元素
---

# No.1299  将每个元素替换为右侧最大元素  

两种解法,两种思路.

## 题目  

给你一个数组 arr ，请你将每个元素用它右边最大的元素替换，如果是最后一个元素，用 -1 替换。

完成所有替换操作后，请你返回这个数组。

示例：
```markdown
输入：arr = [17,18,5,4,6,1]
输出：[18,6,6,6,1,-1]
```

提示：
```javascript
1 <= arr.length <= 10^4
1 <= arr[i] <= 10^5
```

``` javascript
/**
 * @param {number[]} arr
 * @return {number[]}
 */

var replaceElements = function(arr) {

};
```

## 思路
题目比较简单,先想到的就是数组的 reduce : 


## 解答

``` javascript
/**
 * @param {number[]} arr
 * @return {number[]}
 */
var replaceElements = function(arr) {
    let len = arr.length;
    return arr.reduce((acc,item,i)=> {
        let value = -1
        if(i+1 !== len) {
            value = Math.max(...arr.slice(i+1))
        }
        acc.push(value);
        return acc;
    },[])
};
```

## 解析  

整体思路也算是清晰,不太清楚的可以看一下 Array.prototype.reduce() 的使用.  
而另外一种解答呢,就是从后向前,进行比较:

## 另一种解答  

```javascript
var replaceElements = function(arr) {
    let len = arr.length;
    let result = [];
    result[len-1] = -1;
    for(let i = len-2;i>=0;i--) {
        result[i] = Math.max(arr[i+1],result[i+1]);
    }
    return result;
};
```  

## 想法  
主要是考察数组的操作等.  

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/replace-elements-with-greatest-element-on-right-side

