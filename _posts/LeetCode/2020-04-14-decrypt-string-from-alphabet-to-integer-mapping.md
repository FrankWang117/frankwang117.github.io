---
layout: post
title: LeetCode No.1309 解码字母到整数映射
categories: [LeetCode]
description:  No.1309 解码字母到整数映射
keywords: LeetCode, JavaScript, decrypt-string-from-alphabet-to-integer-mapping, 解码字母到整数映射
---

# No.1309 解码字母到整数映射  
  
对字符串的一些考察.  

## 题目  

给你一个字符串 s，它由数字（'0' - '9'）和 '#' 组成。我们希望按下述规则将 s 映射为一些小写英文字符：

字符（'a' - 'i'）分别用（'1' - '9'）表示。
字符（'j' - 'z'）分别用（'10#' - '26#'）表示。 
返回映射之后形成的新字符串。

题目数据保证映射始终唯一。

示例1:  

```markdown
输入：s = "10#11#12"
输出："jkab"
解释："j" -> "10#" , "k" -> "11#" , "a" -> "1" , "b" -> "2".
```  

示例2:  

```markdown
输入：s = "1326#"
输出："acz"
```
提示：
```markdown
 - 1 <= s.length <= 1000
 - s[i] 只包含数字（'0'-'9'）和 '#' 字符。
 - s 是映射始终存在的有效字符串。
```

``` javascript
/**
 * @param {string} s
 * @return {string}
 */
var freqAlphabets = function(s) {

};
```

## 思路  

思路就是进行遍历,然后查看当前字符的后两个字符里面有没有 '#',然后在做字符的转换:

## 解答

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var freqAlphabets = function(s) {
    let result = '';
    
    for(let i = 0,len=s.length;i<len;++i) {
        if(i+2 < len && s.slice(i,i+3).indexOf('#') === 2) {
            result += String.fromCharCode(96 + parseInt(s.slice(i,i+3)))
            i += 2;
        } else {
            result += String.fromCharCode(96 + parseInt(s.slice(i,i+1)))
        }
    }
    return result
};
```

## 解析  

这时间复杂度 O(n).

下面是网上看到的其他答案:

## 另一种解答  

```javascript
var freqAlphabets = function(s) {
    let obj = {"1":"a","2":"b","3":"c","4":"d","5":"e","6":"f","7":"g","8":"h","9":"i","10#":"j","11#":"k","12#":"l","13#":"m","14#":"n","15#":"o","16#":"p","17#":"q","18#":"r","19#":"s","20#":"t","21#":"u","22#":"v","23#":"w","24#":"x","25#":"y","26#":"z"},
        arr = s.match(/\d{1,2}#|\d/g), res = ''
    for(let i=0; i<arr.length; i++){
        res += obj[arr[i]]
    }
    return res
}
```  

## 想法  
时间复杂度没有变化.

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/decrypt-string-from-alphabet-to-integer-mapping/submissions/

