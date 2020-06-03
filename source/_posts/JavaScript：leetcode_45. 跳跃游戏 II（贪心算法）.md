---
title: JavaScript：leetcode_45. 跳跃游戏 II（贪心算法）
date: 2020-05-04 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 跳跃游戏 II, 贪心
---

### 题目说明
```
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

示例:

输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
说明:

假设你总是可以到达数组的最后一个位置。

```
<!-- more -->

### 解题思路一
贪心算法。
首先我们要理解这个题目。
以 `[3,5,2,4,1,1,1,1,1]`为例子

`index`表示我们所处`nums中的位置`，`maxLength`代表我们下一步`最远能到达`的位置。

1. 第0步时，我们处在index = 0，maxLength = 3，表示我们现在最远可以到达index = 3的位置。
2. 当我们选择跳一步时，我们拥有了`5`，`maxLength`就变成了`index（1,5的下标）+ 5 = 6`，相当于从`0-6`的位置尽在我们掌握了
3. 当我们选择跳二步时，我们拥有的是`2`，`maxLength`就变成了`index（2，2的下标）+ 2 = 4`，显然没有6大呀，不可取，不可取。
4. 当我们选择跳三步时，我们拥有的是`4`，`maxLength`就变成了`index（3，4的下标）+4 = 7`，比6大，可以走的更远了，不错不错，就要他了。现在我们掌握了从 `0 - 7` 的位置跳转的权力了，所以它是目前的最优解，包含了其他的所以情况。
5. 这个时候我们的位置就变成了，`index = 3， maxLength = 7`
6. 然后我们判断一下，我们现在的最远距离能不能到达结尾`(maxLength >= (nums.length - 1)) ? `，如果可以，那就结束啦，目前我们走了`1`步，那么`下一步就可以到`了，所以就是`一共要走 1 + 1 步`，返回 `2` 就好啦。
7. 如果不可以，那还得从`步骤 1 到步骤 5` 再来一遍。

上面的过程，表示了我们正常处理的流程，符合我们的的思考习惯。所以问题其实就转化为了，求 可跳跃范围内的`下标index + nums[index]的最大值`，取出这个值，作为下一次的起点。


### 代码实现一
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function(nums) {
	if (nums.length === 1) {
        return 0;
    }
    if ((nums.length - 1) <= nums[0]) {
        return 1;
    }
    let step = 0;
    let index = 0;
    let maxLength = index + nums[index];
    for (let i = 0; i < nums.length;) {
       	index = i;
        for (let j = i + 1; j <= i + nums[i]; j++) {
            if ((j + nums[j]) > maxLength) {
                index = j
                maxLength = (index + nums[index])
            }
        }
        step++;
        i = index;
        if (maxLength >= (nums.length - 1)) {
            return ++step
        }
    }
    return step
};
```