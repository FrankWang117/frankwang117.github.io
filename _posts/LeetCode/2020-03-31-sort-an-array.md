---
layout: post
title: LeetCode No.912 排序数组
categories: [LeetCode]
description: LeetCode No.912 排序数组
keywords: LeetCode, JavaScript, 数组排序, 快速排序
---

# LeetCode No.912 排序数组 
 
常用的排序算法的总结归纳.   

## 题目  

给你一个整数数组 nums，请你将该数组升序排列。 

示例 1：
```markdown
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

示例 2：
```markdown
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```

提示：
```
1 <= nums.length <= 50000
-50000 <= nums[i] <= 50000
```

``` javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortArray = function(nums) {
};
```

## 思路
就根据排序的思想,一个个攻破:

## 快速排序

``` javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortArray = function(nums) {
    if(nums.length < 2) return nums;
    
    let pivot = nums[0];
    let left = [];
    let right = [];
    
    for(let i = 1;i<nums.length;i++) {
        if(nums[i] < pivot) {
            left.push(nums[i])
        }else {
            right.push(nums[i])
        }
    }
    return [...sortArray(left),pivot,...sortArray(right)]
};
```

上述是快速排序的一个简单实现.


>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sort-an-array

