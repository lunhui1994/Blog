---
title: JavaScript：leetcode_72. 编辑距离（动态规划，vue，react的类似diff算法）
date: 2020-05-08 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 编辑距离, 动态规划
---

### 题目说明
```
给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符
 

示例 1：

输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
示例 2：

输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```
<!-- more -->

### 说明
此题是一个最短编辑距离问题，我们在工作中用到工具和框架有很多也是类似的算法。比如`Git`提交，对比差异。`vue`中将更新前的`虚拟dom`改成更新后的虚拟dom（vue中对此做了取舍，有优化）

首先对题意要有个理解：
#### 一
三种操作，`增，删，改`，都是针对`word1`的。但是其实此题目中只要求求出`最短操作`。所以
`word1`和`word2`之间，`操作互换`一样可以达到同样的效果。

比如`word1`为`people`，`word2`为`peopl`，此时，`word1`末尾`删除e` 或者 `word2`末尾`增加e`都可以达到 `word1 == word2` 的效果。 

所以针对`word1`的`增删`操作可以转化为对`word1`或者`word2`的`增`操作
再加上对`word1`的`改`操作
#### 二
假如当word1和word2末尾相同的时候，其实是相当于没有操作。 比如 `people 到peopl` 和`peoplee到people` 二者所需要的操作都是`相同`的。
### 解题思路一
根据题目意思，我们需要找到最少操作数，最少最优，基本上都和贪心及动态规划有关系。此题需要对比word1和word2进行对比操作。先使用动态规划解决。
1. 首先构建一个二维数组用来记录子问题的解。
	
dp     | ""  | **r**  | (ro) **o**  | (ros) **s** 
-------- | -----| -----| -----| -----
""  | 0 | 1 | 2 | 3
**h** | 1
(ho) **o** | 2
(hor)**r**  | 3
 (hors)**s** | 4
(horse)**e**  | 5

> `dp[i][j]` 表示 `i` 到 `j` 所需要的步数，以`dp[2][1]`为例子，表示`“ho”`转换到`“r”` 所需要的操作数

如表，是我们要初始化出来的`dp二维数组`。表内数字，分别代表`竖列`到达`横排`所需要的操作数。
有了dp数组，我们先来操作一次。求出`dp[1][1]`的值。
1. dp[1][1]处，`word1`为`h`，`word2`为`r`。两位`不同`,那么有三种处理方式
	1. `h -> r` 更改`h`,操作数为`1`，修改之后变成了 `r`和`r`，参照`说明中的第二条`，我们再加上`dp[0][0]`即可
	2. `word2`增加`h`变为`rh`,操作数为`1`, `h`和`rh`参照`说明中的第二条`，就变成了`‘’ => "r"`所用的步数，`dp[0][1]`
	3.  `word1`增加`r`变为`hr`,操作数为`1`, `hr`和`r`参照`说明中的第二条`，就变成了`‘h’ => ""`所用的步数，`dp[1][0]`
2. 当我们分析出了以上三种情况后，我们肯定要取最小值作为我们此次dp[1][1]所要求出来的值了。
3. 转换为代码就是 `1 + Math.min(dp[0][0],dp[0][1],dp[1][0])` 
4. 以上是末尾不相同的情况，如果相同，请参照`说明第二条`。实际上就是和横纵各退一步的情况相同
5. 最后我们将`dp[i][j]`看作此次`dp[1][1]`时。实际代码就出来了
```javascript
	dp[i][j] = word1[i-1] === word2[j-1] ? 
	dp[i-1][j-1] : 
	(1 + Math.min(dp[i-1][j],dp[i][j-1],dp[i-1][j-1]))
```

#### 最终完成的dp
dp     | ""  | **r**  | (ro) **o**  | (ros) **s** 
-------- | -----| -----| -----| -----
""  | 0 | 1 | 2 | 3
**h** | 1 | 1 | 2 | 3
(ho) **o** | 2 | 2 | 1 | 2
(hor)**r**  | 3 | 2 | 2 | 2
 (hors)**s** | 4 | 3 | 3 | 2
(horse)**e**  | 5 | 4 | 4 | 3
### 代码实现一
```javascript
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */
var minDistance = function(word1, word2) {
    let length1 = word1.length;
    let length2 = word2.length;

    let dp = new Array(length1 + 1).fill(0).map((item) => {return new Array(length2 + 1).fill(0)});

    for (let i = 0; i < dp.length; i++) {
        dp[i][0] = i;
    }
    for (let i = 0; i < dp[0].length; i++) {
        dp[0][i] = i;
    }
    //初始化工作结束
    for (let i = 1; i <= length1; i++) {
        for (let j = 1; j <= length2; j++) {
            dp[i][j] = word1[i-1] === word2[j-1] ? dp[i-1][j-1] : (1 + Math.min(
                dp[i-1][j], 
                dp[i][j-1],
                dp[i-1][j-1]
            ))
        }
    }
    return dp[length1][length2]

};
```

### 总结
我们知道vue的diff算法被优化到了O(n)而此题我们观察发现除了两层循环对比每个元素，还需要min操作。实际时间复杂度到达了O(n^3)。 那么vue是如何做到的呢？还记得我们循环节点时需要设置的key。通过这个key，我们就可以一一对应前后节点之间的关系。那我们只需要遍历一次节点就可以了。