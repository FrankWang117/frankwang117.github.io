# coding interviews

## 面试题 03 数组中重复的数字  

找出数组中重复的数字。

在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：`[2, 3, 1, 0, 2, 5, 3]`
输出：2 或 3 
 

限制：

2 <= n <= 100000

### 解答
``` javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var findRepeatNumber = function(nums) {
    let map = [];
    for(let i = 0,len=nums.length;i<len;i++) {
        let num = nums[i]
        if(map[num]) {
            return num
        }
        map[num] = true
    }
    return -1;
};
```

## 面试题04. 二维数组中的查找  

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

示例:

现有矩阵 matrix 如下：
``` javascript
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
给定 target = 5，返回 `true`。

给定 target = 20，返回 `false`。

限制：

`0 <= n <= 1000`
`0 <= m <= 1000`

### 题解
``` javascript
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var findNumberIn2DArray = function(matrix, target) {
    let rows = matrix.length;

    if(rows === 0) return false;

    let cols = matrix[0].length;
    
    if(cols === 0) return false

    // 左下角开始寻找
    // let x = rows -1;
    // let y = 0;

    // while(x>=0) {
    //     while(y<cols && matrix[x][y] < target) {
    //         y++
    //     }
    //     while(y<cols && matrix[x][y] === target) {
    //         return true
    //     }
    //     x--;
    // }
    // 右上角开始寻找
    let x = 0;
    let y = cols -1;
    while(x<rows && y >=0) {
        if(matrix[x][y] === target) {
            return true;
        } else if(matrix[x][y] > target) {
            y--
        } else {
            x++
        }
    }
    return false;
};
```