---
title: leetcode_14. 最长公共前缀
date: 2019-09-20 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: 最长公共前缀, leetcode_14
---

### 题目解析：
	编写一个函数来查找字符串数组中的最长公共前缀。

    如果不存在公共前缀，返回空字符串 ""。
### 示例	
示例 1:
```
    输入: ["flower","flow","flight"]
    输出: "fl"
```
示例 2:
```
    输入: ["dog","racecar","car"]
    输出: ""
    解释: 输入不存在公共前缀。
```
<!-- more -->
### 解题思路

    依序判断所有字符串的第N位字符是否一致。一致就加入共同前缀，不一致就跳出返回之前的前缀。
### 解答
```
    var longestCommonPrefix = function(strs) {
        // 首先判断一下边界情况：
        // 1. 当长度为0时，结果必然为"";
        // 2. 当长度为1时，结果必然为"strs[0]";
        if (!strs.length) {return "";};
        if (strs.length == 1) {return strs[0]};
        // 定义公共前缀
        let comStr = "";
        // 循环每个字符串的每个字母,以第一个字符串的长度为准
        for (let i = 0; i < strs[0].length; i++) {
            // 定义每次循环的第一个字符串的字母 当其字符串长度不够时，取值为undefined,所以会判断为不相等跳出循环。
            let item = strs[0][i];
            // 循环数组
            for (let j = 0; j < strs.length; j++) {
                //判断所有字符串的第i个字母是否一致，不一致返回原来的共同前缀。
                if (strs[j][i] != item) {  
                    return comStr;
                }
            }
            //一致的话将该字母加入共同前缀
            comStr += item;
        }
        // 回共同前缀 当所有字符串都一致的情况下才会在此处返回。
        return comStr;
    };

```

### 解题思路二

    对数组中的字符串排序，然后比较最大和最小的字符串的公共前缀。即为数组的公共前缀

### 解答二
```
    var longestCommonPrefix = function(strs) {
        if (!strs.length) {return "";};
        if (strs.length == 1) {return strs[0]};
        let comStr = "";
        strs.sort();
        for (let i = 0; i < strs[0].length; i++) {
            if (strs[0][i] != strs[strs.length - 1][i]) {
                return strs[0].slice(0, (i + 1));
            }
        }
        return strs[0];
    };

```

