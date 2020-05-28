---
title: JavaScript：leetcode_5. 最长回文子串
date: 2020-05-22 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 最长回文子串
---

### 题目说明
```

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"
```

### 解题思路一
1. 遍历字符串，从i开始向左右扩展对比 `i--, i++` 是否相同，过程求出最大值。
2. 以上仅检测奇数回文即：`"cbabc"`而不能检测`cbbc`
3. 对原字符串进行改造例如`"cbbc"` => `"c#b#b#c"`这样就可以以#为中心对比了。（奇数长度例如`"cbabc"` => `c#b#a#b#c`并不会被#影响，所以不用担心破坏了对比结构。）
4. 对比过程中要注意不要让`#b#`把`b#b`这样的情况给顶替了。相同长度时要取末尾为字母的。


### 代码实现一
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    s = s.split('').join('#');
    let max = 0;
    let start = 0;
    let end = 0;
    for (let i = 0; i < s.length; i++) {
        let left = i - 1;
        let right = i + 1;
        while(left >= 0 && right < s.length) {
            if (s.charAt(left) === s.charAt(right)) {
                if (max < (right - left) && s.charAt(right) !== '#') {
                    max = right - left;
                    start = left;
                    end = right;
                }
                right++;
                left--;
            } else {
                break;
            }
        }
    }
    return s.slice(start, end + 1).split('#').join('')
};
```