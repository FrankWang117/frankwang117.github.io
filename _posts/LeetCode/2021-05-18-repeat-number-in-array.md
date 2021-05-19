---
layout: post
title: LeetCode Offer 03. 数组中重复的数字
categories: [LeetCode]
description: 数组中重复的数字
keywords: LeetCode, JavaScript
---

# Offer 03. 数组中重复的数字

## 题目  

找出数组中重复的数字。  

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
 

限制：
`2 <= n <= 100000`   

```typescript
function findRepeatNumber(nums: number[]): number {

};
```

## 思路  

最开始想到的是先排序，在发现相邻的数字之间有没有相同的。但是时间复杂度比较高，使用 JavaScript 数组的 sort 排序，时间复杂度在 O(logn) 上，在遍历一遍，总时间复杂度为 O(logn + n)，不太合适，但是先写下，也就是：

## 第一遍解答

``` typescript
/**
 * @param {number[]} nums
 * @return {number}
 */
var findRepeatNumber = function(nums) {
    const sortNums=nums.sort();
    
    for(let i = 0; i < sortNums.length;i++){
        if(sortNums[i]===sortNums[i+1]){
            return sortNums[i]
        }
    }
};
```

## 解析  

没啥说的，但是有更方便的解答，是时间复杂度将为 O(n)

## 进阶解答  

``` javascript
function findRepeatNumber(nums: number[]): number {
    let newNums = new Set();
    for(let num of nums) {
        if(newNums.has(num)) {
            return num
        }
        newNums.add(num)
    }
    return -1
};
```

使用 Set 结构是唯一的特性，在插入新数据之前检验是否存在。Set 结构的查找和插入时间复杂度均为 O(1)。 

## 进阶解答二  
原地交换：  
题目说明尚未被充分使用，即 在一个长度为 n 的数组 nums 里的所有数字都在 0 ~ n-1 的范围内 。 此说明含义：数组元素的 索引 和 值 是 一对多 的关系。
因此，可遍历数组并通过交换操作，使元素的 索引 与 值 一一对应（即 nums[i] = inums[i]=i ）。因而，就能通过索引映射对应的值，起到与字典等价的作用。
遍历中，第一次遇到数字 xx 时，将其交换至索引 xx 处；而当第二次遇到数字 xx 时，一定有 nums[x] = xnums[x]=x ，此时即可得到一组重复数字。

算法流程：
遍历数组 numsnums ，设索引初始值为 i = 0i=0 :

若 nums[i] = inums[i]=i ： 说明此数字已在对应索引位置，无需交换，因此跳过；
若 nums[nums[i]] = nums[i]nums[nums[i]]=nums[i] ： 代表索引 nums[i]nums[i] 处和索引 ii 处的元素值都为 nums[i]nums[i] ，即找到一组重复值，返回此值 nums[i]nums[i] ；
否则： 交换索引为 ii 和 nums[i]nums[i] 的元素值，将此数字交换至对应索引位置。
若遍历完毕尚未返回，则返回 -1 。

``` typescript
function findRepeatNumber(nums: number[]): number {
    let i = 0;
    while(i < nums.length) {
        if(nums[i] === i) {
            i++
            continue
        }
        if(nums[i] === nums[nums[i]]) {
            return nums[i]
        }
        let tmp = nums[i];
        nums[i] = nums[tmp]
        nums[tmp] = tmp
    }
    return -1
};
```
作者：jyd
链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/mian-shi-ti-03-shu-zu-zhong-zhong-fu-de-shu-zi-yua/
来源：力扣（LeetCode）  

## 想法  

对 JavaScript 中提供的数据结构了解但不会使用。一些奇妙的解题思路还不能够想出来。
