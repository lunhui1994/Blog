---
title: leetcode_200. 岛屿数量 (DFS)
date: 2020-04-20 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 岛屿数量, DFS
---
### 题目说明
```

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1:

输入:
11110
11010
11000
00000
输出: 1
示例 2:

输入:
11000
11000
00100
00011
输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```
<!-- more -->
### 解题思路
1. 遍历二维数组，遍历时只查找===’1‘的情况，设立大陆数量flag初始化为1
2. 找到一块岛屿（===’1‘），此时将flag+1，并将该岛屿标记为flag（2）
3. 深度遍历与该岛屿连接起来的所有岛屿（连起来的其他’1‘）
4. 将其深度遍历到的岛屿标记为flag
5. 深度遍历完成后，继续二维数组的遍历，如果在深度标记后，又找到一个’1‘，那这肯定是第二块大陆的一部分了，重复2-4的步骤。
6. 返回flag-1就是大陆的数量。

### 代码实现
```javascript
let x = [-1,1,0,0]
let y = [0,0,-1,1]
var mark = function(i,j,flag,grid){
    grid[i][j] = flag;
    for (let key = 0; key < 4; key++) {
         (grid[i + x[key]][j + y[key]] === '1') && (mark(i + x[key],j+y[key],flag,grid));
    }
}
var numIslands = function(grid) {
    if (!grid.length){return 0;}
    let flag = 1;
    
    //这一块操作是在二维数组外层包裹了一层’0‘，也就是一层海洋，方便处理边界问题。start
    grid.unshift(new Array(grid[0].length).fill('0'));
    grid.push(new Array(grid[0].length).fill('0'));
    grid.map((item)=>{
        item.unshift('0')
        item.push('0')
    })
    //这一块操作是在二维数组外层包裹了一层’0‘，也就是一层海洋，方便处理边界问题。end
    
    for(let i = 1; i < grid.length-1; i++) {
        for (let j = 1; j < grid[i].length-1; j++) {
            if (grid[i][j] === '1') {
                flag++;
                mark(i,j,flag,grid);
            }
        }
    }
    return (flag - 1)
};
```