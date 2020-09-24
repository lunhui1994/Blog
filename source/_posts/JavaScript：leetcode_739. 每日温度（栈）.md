---
title: JavaScript：leetcode_739. 每日温度（栈）
date: 2020-06-11 11:27:28
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_739, 每日温度（栈）
--- 
### 题目说明
```
根据每日 气温 列表，请重新生成一个列表，对应位置的输出是需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 0 来代替。

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。

```
暴力方法就不提了，直接循环判断即可。

### 解题思路一（栈）
1. 首先`i`位的值，为`j-i`的差值。`j`为下一位比`i`处大的`最小下标`。
2. 建立一个index栈，先入栈下标0，遍历T，判断`T[i]`和`T[index[index.length-1]]` (栈顶元素对应的T内值)。
3. 若`T[i] >  T[index[index.length-1]]` 将栈顶排出`indexTop = index.pop()`; 赋值`T[indexTop] = i - indexTop` 。
4. 循环此步骤直到栈顶对应的T值大于T[i]。
5. 最后将`i`置入栈内。
6. 遍历完数组，最后将栈排空，栈内剩余值都为0。（我们也可以初始化结果数组为0）
	

### 代码实现一
```javascript
/**
 * @param {number[]} T
 * @return {number[]}
 */
var dailyTemperatures = function(T) {
    let res = new Array(T.length).fill(0);
    let index = [0];
    for (let i = 1; i < T.length; i++) {
        while (T[i] > T[index[index.length - 1]]) {
        	let indexTop = index.pop();
            res[indexTop] = i - indexTop;
        }
        index.push(i);
    }
    return res;
};
```
