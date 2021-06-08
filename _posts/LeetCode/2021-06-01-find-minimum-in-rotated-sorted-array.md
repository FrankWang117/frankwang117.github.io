---
layout: post
title: LeetCode Offer 11. 旋转数组的最小数字
categories: [LeetCode]
description: 旋转数组的最小数字
keywords: LeetCode, JavaScript, array, 11
---

# Offer 11. 旋转数组的最小数字

## 题目

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组  [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为 1。

示例 1：

```
输入：[3,4,5,1,2]
输出：1
示例 2：

输入：[2,2,2,0,1]
输出：0
```

> 注意：本题与主站 [154 题相同](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

## 解答

```typescript
function minArray(numbers: number[]): number {
  if (!Array.isArray(numbers)) {
    throw Error("wrong type");
  }
  let start = 0,
    end = numbers.length - 1;
  while (start < end) {
    if (numbers[end] > numbers[start]) {
      return numbers[start];
    }
    let middle = Math.ceil((start + end) / 2);
    if (numbers[middle] > numbers[end]) {
      start = middle + 1;
    } else if (numbers[middle] < numbers[end]) {
      end = middle;
    } else {
      ++start;
    }
  }
  return numbers[start];
}
```

## 解析

先是错误处理，不多描述了。  
然后总体的一个思路是将数组认为是由两个递增数组连接在一起的：左部递增数组，以及右部递增数组。
取中间值(nums[middle])，与数组头(nums[start])以及数组尾部(nums[end])进行比较，并缩短最小值所在数组的范围：

- 当末尾值大于初始值时，此数组旋转的个数为 0 个，即是其本身，返回 index 为 0 的值；
- 当数组的中间下标(middle)的值大于末尾值时，说明中间值在左部递增数组中，初始值的下标改为当前中间值的下标后一位，即 start = middle + 1；
- 当数组的中间下标(middle)的值小于末尾值时，说明中间值在右部递增数组中，末尾值的下标改为当前中间值的下标，即 end = middle；
- 其他情况下，如 [2,2,2,2,2,1,2] 这种情况需要递增初始值下标，直到 start > end 结束循环。

## 想法

主要是其中一些逻辑的判断。
