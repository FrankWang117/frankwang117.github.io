---
layout: post
title: LeetCode No.832 翻转图像
categories: [LeetCode]
description:  No.832 翻转图像
keywords: LeetCode, JavaScript, flipping-an-image, 翻转图像
---

# No.832 翻转图像  
  
使用数组的一些方法来解决这个问题.或者不利用现成的 API 解决.

## 题目  

给定一个二进制矩阵 A，我们想先水平翻转图像，然后反转图像并返回结果。

水平翻转图片就是将图片的每一行都进行翻转，即逆序。例如，水平翻转 [1, 1, 0] 的结果是 [0, 1, 1]。

反转图片的意思是图片中的 0 全部被 1 替换， 1 全部被 0 替换。例如，反转 [0, 1, 1] 的结果是 [1, 0, 0]。  

示例1:  

```markdown
输入: [[1,1,0],[1,0,1],[0,0,0]]
输出: [[1,0,0],[0,1,0],[1,1,1]]
解释: 首先翻转每一行: [[0,1,1],[1,0,1],[0,0,0]]；
     然后反转图片: [[1,0,0],[0,1,0],[1,1,1]];
```  

示例2:  

```markdown
输入: [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
输出: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
解释: 首先翻转每一行: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]]；
     然后反转图片: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
```
提示：
```markdown
 - 1 <= A.length = A[0].length <= 20;
 - 0 <= A[i][j] <= 1。
```

``` javascript
/**
 * @param {number[][]} A
 * @return {number[][]}
 */
var flipAndInvertImage = function(A) {

};
```

## 思路  
这问题明显的是要使用 `Array.prototype.reduce` 来解决问题呀: 

## 解答

```javascript
/**
 * @param {number[][]} A
 * @return {number[][]}
 */
var flipAndInvertImage = function(A) {
    return A.reduce((acc,cur) => {
        acc.push(cur.reverse().map( ele => ele?0:1))
        return acc;
    },[])
};
```

## 解析  

很好理解,先 reverse 再翻转.但是时间上不够好.

下面是网上看到的其他答案:

## 另一种解答  

```javascript
/**
 * @param {number[][]} A
 * @return {number[][]}
 */
var flipAndInvertImage = function (A) {
    for (let i = 0; i < A.length; i++) {
        let len = A[i].length - 1
        // len >> 1 跟 parseInt(len / 2)一个效果
        for (let j = len >> 1; j >= 0; j--) {
            let left = 1 ^ A[i][len - j];
            let right = 1 ^ A[i][j];
            A[i][len - j] = right;
            A[i][j] = left;
            // 这种是解构写法，但实际测试出来内存大于上面的。装逼必备
            // [A[i][len - j], A[i][j]] = [1 ^ A[i][j], 1 ^ A[i][len - j]]
        }
    }
    return A;
};

作者：zzx0106
链接：https://leetcode-cn.com/problems/flipping-an-image/solution/bu-yi-lai-reverse-by-zzx0106/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```  

## 想法  
第二种思路还是要好好看一下...

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/flipping-an-image/

