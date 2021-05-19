---
layout: post
title: LeetCode Offer 04. 二维数组中的查找
categories: [LeetCode]
description: 二维数组中的查找
keywords: LeetCode, JavaScript
---

# Offer 04. 二维数组中的查找

## 题目  

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。  

示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。

 

限制：
`0 <= n <= 1000`,`0 <= m <= 1000`


```typescript
function findNumberIn2DArray(matrix: number[][], target: number): boolean {

};
```

## 思路  

想到可以用二分法，在开始的时候向上取整 n/2，m/2，对比这个位置的二维数组的值与目标值，根据情况再拆分二维数组，但是拆分遇到问题，没有解答

## 正确解答

``` typescript
function findNumberIn2DArray(matrix: number[][], target: number): boolean {
    if(!Array.isArray(matrix) || matrix.length === 0) {
        return false;
    }
    let row = 0;
    let column = matrix[0].length - 1;
    while(row < matrix.length && column >-1) {
        if(matrix[row][column] === target) {
            return true
        } else if(matrix[row][column] > target) {
            column--
        } else {
            row++
        }
    }
    return false;
};
```

## 解析  

利用有序矩阵的特点，从矩阵的右上角开始判断与目标值的大小，大于的话向当前行左侧递减，小于的话向当前列下方递增。

## 其他解答 

看了下其他的解答，其实可以通过两遍遍历，查找
``` typescript
function findNumberIn2DArray(matrix: number[][], target: number):boolean {
        if (!Array.isArray(matrix) || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        let rows = matrix.length, columns = matrix[0].length;
        for (let i = 0; i < rows; i++) {
            for (let j = 0; j < columns; j++) {
                if (matrix[i][j] == target) {
                    return true;
                }
            }
        }
        return false;
    }
}

```
