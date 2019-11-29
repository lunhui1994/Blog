---
title: leetcode_1189. “气球” 的最大数量
date: 2019-09-25 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_1189
---
### 题目解析：
给你一个字符串 text，你需要使用 text 中的字母来拼凑尽可能多的单词 "balloon"（气球）。

字符串 text 中的每个字母最多只能被使用一次。请你返回最多可以拼凑出多少个单词 "balloon"。
### 示例	
示例 1:
```
    输入：text = "nlaebolko"
    输出：1
```
示例 2:
```
    输入：text = "loonbalxballpoon"
    输出：2
```
提示：
```
    1 <= text.length <= 10^4
    text 全部由小写英文字母组成
```
<!-- more -->
### 解题思路

    先找出各个字母的个数，然后找出其中的最小值(o,l数量除以2)。
    用match正则检测字符串中符合条件的字母，其长度即为该字母在字符串中的个数。
### 解答
```
    var maxNumberOfBalloons = function(text) {
        let regexp, singleNum, min = text.length;
        for (let i in "balon") {
            regexp = new RegExp("balon"[i], 'g');
            singleNum = text.match(regexp).length;
            if ("balon"[i]) == 'o' || "balon"[i]) == 'l') {
                singleNum = Math.floor(singleNum / 2)
            }
            min = (min <= singleNum) ? min : singleNum;
        }
        return min;
    };
```

### 解题思路二

    用split分割字符串，分割之后的数组长度-1 就是该字符(分隔符)在该字符串中的个数。

### 解答二
```
    var maxNumberOfBalloons = function(text) {
        let singleNum, min = text.length;
        for (let i in "balon") {
            singleNum = text.split("balon"[i]).length - 1;
            if ("balon"[i]) == 'o' || "balon"[i]) == 'l') {
                singleNum = Math.floor(singleNum / 2)
            }
            min = (min <= singleNum) ? min : singleNum;
        }
        return min;
    };

```

