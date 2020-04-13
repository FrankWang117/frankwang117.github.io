---
layout: post
title: LeetCode No.1304 和为零的N个唯一整数
categories: [LeetCode]
description:  No.1304 和为零的N个唯一整数
keywords: LeetCode, JavaScript, find-n-unique-integers-sum-up-to-zero
---

# No.1304 和为零的N个唯一整数  
  
这个题目的结果不唯一,全看自己的思路.  

## 题目  

给你一个整数 n，请你返回 任意 一个由 n 个 各不相同 的整数组成的数组，并且这 n 个数相加和为 0 。

示例1：
```markdown
输入：n = 5
输出：[-7,-1,1,3,4]
解释：这些数组也是正确的 [-5,-1,1,2,3]，[-3,-1,2,-2,4]。
```

示例：
```markdown
输入：n = 3
输出：[-1,0,1]
```  


提示：
```javascript
1 <= n <= 1000
```

``` javascript
/**
 * @param {number} n
 * @return {number[]}
 */
var sumZero = function(n) {

};
```

## 思路
首先想到的就是一正一负,如果是奇数,那就加个零不就可以了么.


## 解答

``` javascript
var sumZero = function(n) {
    let result = Array(n).fill(0),
        index = 0;
    
    for (let i = 1;i<= n/2;i++) {
        result[index++] = i;
        result[index++] = -i;
    }
    return result;
}
```

## 解析  

这个比较好理解,就是初始化一个子项全为 0 的数组,然后按照一正一负的数组放进去.  
当然,还有其他的思路,比如说,可以按照 1 - n-1,然后第 0 项直接是后面所有项的和 * -1.

## 另一种解答  

```javascript
/**
 * @param {number} n
 * @return {number[]}
 */
var sumZero = function(n) {
    let result = Array(n);
    for( let i = 1;i<n;i++) {
        result[i] = i
    }
    let total = -((1+n)* (n/2) - n);
    result[0] = total;
    return result
};
```  

## 想法  
就是思维能力的考察吧.感觉还是比较有意思的,就记录下来了. 

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-n-unique-integers-sum-up-to-zero/

