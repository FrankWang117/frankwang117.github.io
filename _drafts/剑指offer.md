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

## 面试题10- II. 青蛙跳台阶问题  

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

示例 1：

输入：`n = 2`
输出：`2`

示例 2：

输入：`n = 7`
输出：`21`
提示：`0 <= n <= 100`

### 题解
``` javascript
/**
 * @param {number} n
 * @return {number}
 */
var numWays = function(n) {
    // answer 1
    // if(n == 0 || n ==1) {
    //     return 1
    // };
    // if(n ==2) {
    //     return 2
    // }
    // return (numWays(n-1) + numWays(n-2))% (1e9+7)
    // answer2
    // let arr = Array(n);
    // arr[0] = 1;
    // arr[1] = 1;
    // arr[2] = 2;
    // for(let i = 3;i<=n;i++) {
    //     arr[i] = (arr[i-1]+ arr[i-2])%(1e9+7)
    // }
    // return arr[n]
    // answer3
    let a = 1,b = 1,sum;
    for(let i = 0;i < n;i++) {
        sum = (a + b) %(1e9+7);
        a = b;
        b = sum;
    }
    return a
};
```
## 面试题11. 旋转数组的最小数字  

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例 1：

输入：`[3,4,5,1,2]`
输出：`1`  

示例 2：

输入：`[2,2,2,0,1]`
输出：`0`

### 解题
``` javascript
/**
 * @param {number[]} numbers
 * @return {number}
 */
var minArray = function(numbers) {
    let len = numbers.length;

    if(len === 0) {
        return 0
    }
    let left = 0;
    let right = len - 1;
    while(left < right) {

        let mid = Math.floor((left + right) / 2);
        if(numbers[left]<numbers[right]) {
            return numbers[left]
        }
        if(numbers[left] < numbers[mid]) {
            left = mid + 1
        } else if (numbers[mid] < numbers[right]) {
            right = mid
        } else {
            ++left
        }
    }
    return numbers[left]
};
```  

## 面试题14- I. 剪绳子

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m] 。请问 k[0]*k[1]*...*k[m] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

示例 1：

输入: `2`
输出: `1`
解释: `2 = 1 + 1, 1 × 1 = 1`
示例 2:

输入: `10`
输出: `36`
解释: `10 = 3 + 3 + 4, 3 × 3 × 4 = 36`
提示：`2 <= n <= 58` 

``` javascript
/**
 * @param {number} n
 * @return {number}
 */
var cuttingRope = function(n) {
    //动态规划
    // let dp = Array(n+1).fill(1);

    // for(let i = 3;i<=n;i++) {
    //     for(let j = 1;j<i;j++) {
    //         dp[i] = Math.max(dp[i],j*(i-j),j*dp[i-j])
    //     }

    // }
    // return dp[n]
    // 数学规律
    if(n === 2) return 1;
    if (n === 3) return 2;

    let a = Math.floor(n / 3);
    let b = n % 3;
    if(b === 0) return Math.pow(3,a);
    if(b === 1) return Math.pow(3,a-1)*4
    return Math.pow(3,a)*2
};
```

## 面试题14- II. 剪绳子 II

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m] 。请问 k[0]*k[1]*...*k[m] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。


示例 1：

输入: `2`
输出: `1`
解释: `2 = 1 + 1, 1 × 1 = 1`
示例 2:

输入: `10`
输出: `36`
解释: `10 = 3 + 3 + 4, 3 × 3 × 4 = 36`
 

提示：`2 <= n <= 1000`

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var cuttingRope = function(n) {
    if(n===2) return 1;
    if(n===3) return 2;
    let times = 1;
    while(n>4) {
        times *= 3
        times = times %( 1e9+7)
        n -= 3
    }
    return times * n % ( 1e9+7)
};
```