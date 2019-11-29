---
title: leetcode_26. 删除排序数组中的重复项
date: 2019-09-27 11:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode_26, 删除排序数组中的重复项
---
### 题目解析：
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

### 示例	
示例 1:
```
    给定数组 nums = [1,1,2], 

    函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

    你不需要考虑数组中超出新长度后面的元素。
```
示例 2:
```
    给定 nums = [0,0,1,1,1,2,2,3,3,4],

    函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

    你不需要考虑数组中超出新长度后面的元素。

```

提示：

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:
```
    // nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
    int len = removeDuplicates(nums);

    // 在函数里修改输入数组对于调用者是可见的。
    // 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
    for (int i = 0; i < len; i++) {
        print(nums[i]);
    }
```
<!-- more -->
### 解题思路
    题目中原地的意思大概就是要在原数组中操作不能开辟新的数组空间。
    题目简单，就是去除数组中的重复元素。
    首先想到的是删除数组中的重复元素，用到了splice。

### 解答
```
    var removeDuplicates = function(nums) {
        for (var i = 0; i < nums.length; i++) {
            if (nums.includes(nums[i], i + 1)) {
                nums.splice(i, 1);
                i--;
            }
        }
        return nums.length;
    };
```

### 解题思路二

    由于第一种方法用到了splice，所以其实时空间复杂度还是比较高的。
    所以，根据题目要求我们其实只需要保证数组的前面排列的是我们需要的就可以了。超过的部分我可以忽略不计。
    那其实就是依次从头填充数组就可以了！直到遍历完数组的最后一位。
    双指针就解决了。一个用来遍历数组，一个用来从头修改数组。

    由于题目告诉为排序数组，所以我们可以用i，i+1判断。
### 解答二
```
var removeDuplicates = function(nums) {
    var next = 0;
    for (var i = 0; i < nums.length; i++) {
        if (nums[i] != nums[i+1]) {
            nums[next] = nums[i];
            next++;
        }
    }
    return next;
};

```


### 解题思路三

    了解一下ES6的array.includes(searchEle, fromIndex);
    判断数组种是否含有searchEle。(true / false);
    searchEle为搜索的元素（必填），fromIndex为从数组的哪一位开始搜索。
    
    使用includes同时可以判断非排序的数组

### 解答三
```
var removeDuplicates = function(nums) {
    var j = 0;
    for (var i = 0; i < nums.length; i++) {
        if (!nums.includes(nums[i], i + 1)) {
            nums[j] = nums[i];
            j++;
        }
    }
    return j;
};

```