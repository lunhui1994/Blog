---
title: leetcode_11. 盛最多水的容器 （暴力遍历=>双指针）
date: 2020-04-19 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 盛最多水的容器
---

### [题目描述](https://leetcode-cn.com/problems/container-with-most-water/)

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

 
### 解题思路 一暴力法
办法一很简单，双层循环计算出所有可能的结果，选出最大的。


```javascript


var maxArea = function(height) {
    let area = 0;
    for (let i = 0; i < height.length; i++) {
        for(let j = i; j < height.length; j++) {
            let m2 = Math.min(height[i],height[j]) * (j-i);
            if (m2 > area) {
                area = m2;
            }
        }
    }
    return area;
};

```
一点难度都没有。

### 优化

接下来我们优化一下


```javascript
var getArea = function(height, i, j){
    return Math.min(height[i],height[j]) * (j-i);
}
var maxArea = function(height) {
    let start = 0;
    let end = height.length - 1;
    let area = getArea(height, start, end );
    for (let i = 0; i < height.length; i++) {
        for(let j = height.length - 1; j > i; j--) {
            if (height[j] < height[end]) {
                continue;
            }
            if (area < getArea(height, i, j)) {
                area = getArea(height,i, j);
                start = i;
                end = j;
            }
        }
    }
    return area;
};
```

1. 把计算面积提出到getArea中了
2. 标记了start，end。并利用其进行剪枝（优化去掉不必要的for循环），因为我们很容易看出来，比end（距离start最远）位更短的木板没有必要再进行计算了。

### 解题思路 二 双指针
其实这道题的思路是这样：
计算start，end之间的距离（宽），和Math.min(start，end)(高)的面积。
怎么计算最大面积呢？目标是找最宽和最高。
即在start，end距离最远的情况下，找两边最高的木板。

所以，我们通过让start和end在最远的距离处，开始慢慢靠近，靠近的规则是：
1. 抛弃最短的，即若start比end处的木板短，start向右移一位+1，（反之end处短，则end - 1）
2. 这样可以使两边都保证是最长的木板之间的面积
3. 同样我们可以加入上面的优化，移位后，比原本位置短的，直接过滤掉。


```javascript


var getArea = function(height, i, j){
    return Math.min(height[i],height[j]) * (j-i);
}
var maxArea = function(height) {
    let start = 0;
    let end = height.length - 1;
    let area = getArea(height, start, end );
    for (let i = 0,j = end; i < j;) {
            if (height[i] > height[j]) {
                j--;
                if (height[j] < height[end]) {
                    continue;
                }
            } else {
                i++
                if (height[i] < height[start]) {
                    continue;
                }
            }
            let m2 = getArea(height, i, j);
            if (area < m2) {
                area = m2;
                start = i;
                end = j;
            }
    }
    return area;
};

```