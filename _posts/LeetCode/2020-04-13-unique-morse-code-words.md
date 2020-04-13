---
layout: post
title: LeetCode No.804 唯一摩尔斯密码词
categories: [LeetCode]
description:  No.804 唯一摩尔斯密码词
keywords: LeetCode, JavaScript, unique-morse-code-words, 唯一摩尔斯密码词
---

# No.804. 唯一摩尔斯密码词  
  
使用了字符串的一些操作,以及 hash 表用来处理唯一的问题.  

## 题目  

国际摩尔斯密码定义一种标准编码方式，将每个字母对应于一个由一系列点和短线组成的字符串， 比如: "a" 对应 ".-", "b" 对应 "-...", "c" 对应 "-.-.", 等等。

为了方便，所有26个英文字母对应摩尔斯密码表如下：

[".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."]
给定一个单词列表，每个单词可以写成每个字母对应摩尔斯密码的组合。例如，"cab" 可以写成 "-.-..--..."，(即 "-.-." + "-..." + ".-"字符串的结合)。我们将这样一个连接过程称作单词翻译。

返回我们可以获得所有词不同单词翻译的数量。  

```markdown
例如:
输入: words = ["gin", "zen", "gig", "msg"]
输出: 2
解释: 
各单词翻译如下:
"gin" -> "--...-."
"zen" -> "--...-."
"gig" -> "--...--."
"msg" -> "--...--."

共有 2 种不同翻译, "--...-." 和 "--...--.".
```

提示：
```markdown
 - 单词列表words 的长度不会超过 100。
 - 每个单词 words[i]的长度范围为 [1, 12]。
 - 每个单词 words[i]只包含小写字母。
```

``` javascript
/**
 * @param {string[]} words
 * @return {number}
 */
var uniqueMorseRepresentations = function(words) {
    
};
```

## 思路
显示将数组中的所有的字符串通过便利操作转换为摩尔斯码,然后使用 hash 去掉重复的摩尔斯码并记录摩尔斯码出现的次数:  

## 解答

/**
 * @param {string[]} words
 * @return {number}
 */
var uniqueMorseRepresentations = function(words) {
    const table = [".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."];
    let result = {},
        resNum = 0;
    
    for( let i = 0,len = words.length; i<len; i++ ) {
        let curStr = ''
        for(let j = 0,slen = words[i].length;j<slen;j++) {
            curStr += table[words[i].charCodeAt(j) - 97]
        }
        if(!result[curStr]) {
            resNum++
            result[curStr] = true;
        }
    }
    return resNum;
};
```

## 解析  

这个的执行效率就比较低了,虽然空间复杂度能达到不错.  

下面是网上看到的其他答案:

## 另一种解答  

```javascript
/**
 * @param {string[]} words
 * @return {number}
 */
var uniqueMorseRepresentations = function(words) {
    const codeList = [".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."];
    const letterList = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p','q', 'r', 's', 't', 'u', 'v', 'w', 'x','y', 'z']
    let letter2CodeMap = {};
    codeList.forEach((item, idx) => {
        letter2CodeMap[letterList[idx]] = item;
    });
    let translatedList = [];
    words.forEach((item) => {
        let str = '';
        for (let i = 0; i < item.length; i ++) {
            str += letter2CodeMap[item[i]];
        }
        translatedList.push(str);
    })
    return [...new Set(translatedList)].length;
};

作者：front-end-coder
链接：https://leetcode-cn.com/problems/unique-morse-code-words/solution/jian-li-zi-mu-yu-mi-ma-de-ying-she-shu-zu-qu-zhong/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```  

## 想法  
就是思维能力的考察吧.感觉还是比较有意思的,就记录下来了. 

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-morse-code-words/

