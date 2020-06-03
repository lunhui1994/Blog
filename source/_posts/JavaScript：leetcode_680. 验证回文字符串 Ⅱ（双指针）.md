---
title: JavaScript：leetcode_680. 验证回文字符串 Ⅱ（双指针）
date: 2020-05-19 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 验证回文字符串 Ⅱ, 双指针
---

### 题目说明
```

给定一个非空字符串 s，最多删除一个字符。判断是否能成为回文字符串。

示例 1:

输入: "aba"
输出: True
示例 2:

输入: "abca"
输出: True
解释: 你可以删除c字符。
注意:

字符串只包含从 a-z 的小写字母。字符串的最大长度是50000。
```
<!-- more -->

### 解题思路一
1. 回文字符串，字符串`正序反序都一样`。同样也是`对称`的。
2. 正反指针，一个从`头`，一个从`末尾`，对比。
3. 找到不同的位置，去掉该位置的值。（可能为`i`，也可能为`length-1-i`）
4. 若两种情况中`有一种`是回文。那就返回`true`。否则返回`false`
5. 找不到不同的值当然也返回`true`


### 代码实现一
```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var validPalindrome = function(s) {
    let sArr = s.split('');
    for (let i = 0; i <= (sArr.length >> 1); i++) {
        if (sArr[i] !== sArr[sArr.length - i - 1]) {
            let f = [...sArr];
            f.splice(i,1);
            let f2 = [...sArr]; 
            f2.splice(sArr.length - i - 1,1);
            if ((f+'') == ([...f].reverse()+'') || (f2 + '') == ([...f2].reverse() + '')) {
                return true
            } else {
                return false
            }
        }
    }
    return true
};
```