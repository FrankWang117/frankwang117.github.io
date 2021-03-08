---
layout: post
title: 选择排序的 JavaScript 实现
categories: [算法]
description: 选择排序的 JavaScript 实现
keywords: algorithm, 算法, 排序, 选择排序
---

用 JavaScript 实现选择排序。

# 选择排序

## 思路

通过遍历 `arr.length - 1` 个数，把第一个没有排序过的元素设置为最小值，然后通过遍历余下的没有
遍历过的元素，如果元素的值 < 现在的最小值，将此元素设置为新的最小值，将最小值和第一个没有排序过
的元素的位置交换。  
返回数组。

## 动图示例

![select sort](https://raw.githubusercontent.com/FrankWang117/images/master/选择排序.gif)

## 具体实现 - JavaScript

```javascript
function select(arr) {
  if (arr.length === 0 || !Array.isArray(arr)) {
    return arr;
  }
  for (let i = 0, len = arr.length; i < len - 1; i++) {
    let index = i;
    for (let j = i + 1; j < len; j++) {
      if (arr[index] > arr[j]) {
        index = j;
      }
    }
    if (index != i) {
      let tmp = arr[i];
      arr[i] = arr[index];
      arr[index] = tmp;
    }
  }
  return arr;
}
```
