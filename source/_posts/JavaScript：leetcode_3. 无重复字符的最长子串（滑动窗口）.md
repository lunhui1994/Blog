---
title: JavaScript：leetcode_3. 无重复字符的最长子串（滑动窗口）
date: 2020-05-02 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 无重复字符的最长子串, 滑动窗口
---

### 题目说明
```

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

```
<!-- more -->

这个题是我昨天晚上做的，没想到今天变成每日一题了。。我这个也算神预言了~
### 解题思路一
通过滑动窗口去解决它，
1. 建立一个空字符串str，遍历原始字符串s每个位置的值s[i]
2. 若str中不存在s[i]，str+=s[i]
3. 若存在，滑动窗口，寻找str中s[i]的位置str.indexOf(s[i])，截取该位置之后的str部分，然后str+=s[i].
4. 进行第三步时，需要取max(length, str.length) ，length为之前截取时记录的str.length的最大值。
5. 最后返回最大值即可。
### 代码实现一
```javascript
var lengthOfLongestSubstring = function(s) {
    let str = '';
    let length = 0;
    for (let i = 0; i < s.length; i++) {
       if (str.indexOf(s[i]) == -1) {
           str += s[i]
       } else {
           length = Math.max(length, str.length);
           str = str.slice(str.indexOf(s[i]) + 1) + s[i];
       }
    }
    return  Math.max(length, str.length);
};
```


