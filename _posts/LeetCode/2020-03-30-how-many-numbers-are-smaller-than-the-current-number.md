---
layout: post
title: LeetCode No. 1365 有多少小于当前数字的数字 
categories: [LeetCode]
description: No. 1365 有多少小于当前数字的数字 
keywords: LeetCode, JavaScript, 有多少小于当前数字的数字
---

# No. 1365 有多少小于当前数字的数字  

不使用常规的方法,如何解答呢?引出了一个计数排序思想.  

## 题目  
给你一个数组 nums，对于其中每个元素 nums[i]，请你统计数组中比它小的所有数字的数目。

换而言之，对于每个 nums[i] 你必须计算出有效的 j 的数量，其中 j 满足 j != i 且 nums[j] < nums[i] 。

以数组形式返回答案。

 

示例 1：

```markdown
输入：nums = [8,1,2,2,3]
输出：[4,0,1,1,3]
解释： 
对于 nums[0]=8 存在四个比它小的数字：（1，2，2 和 3）。 
对于 nums[1]=1 不存在比它小的数字。
对于 nums[2]=2 存在一个比它小的数字：（1）。 
对于 nums[3]=2 存在一个比它小的数字：（1）。 
对于 nums[4]=3 存在三个比它小的数字：（1，2 和 2）。
```

示例 2：
```markdown
输入：nums = [6,5,4,8]
输出：[2,1,0,3]
示例 3：

输入：nums = [7,7,7,7]
输出：[0,0,0,0]
```

提示：
```
2 <= nums.length <= 500
0 <= nums[i] <= 100
```

``` javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var smallerNumbersThanCurrent = function(nums) {

};
```

## 思路
那首先想到的必然是暴力解法,使用 map 遍历,并使用 filter 寻找出符合条件的元素:  

## 第一遍解答

``` javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var smallerNumbersThanCurrent = function(nums) {

    return nums.map((num,i,arr)=> {
        return arr.filter(item=>item<num).length
    });
};
```

通俗易懂,还占用时间久.不过出现 map 的地方总可以尝试用 reduce 来写一遍,加深对 reduce 的印象:

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var smallerNumbersThanCurrent = function(nums) {
    return nums.reduce((acc,cur)=> {
        acc.push(nums.filter(nums=>nums<cur).length);
        return acc;
    },[]);
};

```
以上两种解法就是一种解法,时间复杂度都是 O(n^2).这时候就要想是不是有更好的方法?比如使用哈希表的一些
方式,能让这些数字出现的个数有个对应:

```javascript
var smallerNumbersThanCurrent = function(nums) {
    let arr = Array(101).fill(0);
    nums.forEach(num => arr[num]++ );
    for(let i = 1;i<101;i++) {
        arr[i] = arr[i] + arr[i-1]
    };
    return nums.map(num => {
        if(num > 0) {
            return arr[num-1]
        }
        return 0
    })
};
```
## 解析  
首先是注意到了题目的提示部分,所以可以建立一个数组,用来表示 nums 里面数字出现的次数.
也就是:
```javascript
let arr = Array(101).fill(0);
nums.forEach(num => arr[num]++ );
```
这两行所做的事情.  
继续向下的代码表明我们将 i 之前的数字出现的次数累加起来:
```javascript
for(let i = 1;i<101;i++) {
        arr[i] = arr[i] + arr[i-1]
    };
```
这时我们获取到的 arr 数组,每一项 arr[i] 表示的就是在 nums 中小于或等于值为 i 的数字的个数.
所以最后我们将其中出现的值取出来即可.  

## 想法  
哈希表在面试中出现的概率还是很大的.能很有效的减少时间复杂度.  

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/how-many-numbers-are-smaller-than-the-current-number

