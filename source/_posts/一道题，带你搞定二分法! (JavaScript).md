---
title: JavaScript:一道题，带你搞定二分法! 
date: 2020-04-29 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, 全排列, 二分法
---


### 题目描述
题目有点长，我就截个图展示了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429104354570.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2x1bmh1aTE5OTRf,size_16,color_FFFFFF,t_70)


### 解题思路
这跟我之前做的那个旋转数组有相似之处，都是两个有序序列的组合。
[JavaScript：leetcode_33. 搜索旋转排序数组（二分法）](https://blog.csdn.net/lunhui1994_/article/details/105791551)

看题目限制，肯定又是不能用遍历的O(n)。而且对获取mountain的值有100次的限制。

那么自然就想到了**二分法**，结合题目，mountain长度为10000那么大概分十几次就完事儿了。基本不会把一百次用完。

我的思路是，用**二分法**找到mountain的山顶**top**，将其分为**两个有序**序列，然后分别用**二分法**查找。

最终算法时间复杂度为 O(log n)

1. findTop 查找山顶top
2. findLeftTarget 左序查找target，左序列为升序序列。
3. findRightTarget 右序列查找target，右序列为降序序列。(也只是在判断条件上有所区别)

如果不太清楚二分法,那么请先看一下 [**文末扩展**](#kz) 
###  题解：
```javascript
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * function MountainArray() {
 *     @param {number} index
 *     @return {number}
 *     this.get = function(index) {
 *         ...
 *     };
 *
 *     @return {number}
 *     this.length = function() {
 *         ...
 *     };
 * };
 */

/**
 * @param {number} target
 * @param {MountainArray} mountainArr
 * @return {number}
 */
var findInMountainArray = function(target, mountainArr) {
    let length = mountainArr.length();
    let left_v = mountainArr.get(0);
    let right_v = mountainArr.get(length - 1);
    if (target < left_v && target < right_v) {
        return -1
    }
    let resMid = findTop(0, length - 1, mountainArr)
    let top = resMid.mid;
    let topV = resMid.mid_v;
    if (target > topV) {
        return -1;
    }
    let left_target = findLeftTarget(0, top, mountainArr, target);
    return left_target === -1 ? findRightTarget(top, length-1, mountainArr, target) : left_target;
};
var findTop = function(left, right, mountainArr) {
    let mid = left + ((right - left) >> 1);
    let mid_lv = mountainArr.get(mid - 1);
    let mid_v = mountainArr.get(mid);
    let mid_rv = mountainArr.get(mid + 1);
    if (mid_v > mid_lv) {
        if (mid_v > mid_rv) {
            return {
                mid: mid,
                mid_v: mid_v,
            }
        } else {
            return findTop(mid, right, mountainArr)
        }
    } else if (mid_v < mid_lv) {
        return findTop(left, mid, mountainArr)
    }
}
var findLeftTarget = function(left, right, mountainArr, target) {
    if (right - left <= 1) {
        let left_v = mountainArr.get(left);
        if (left_v === target) {
            return left;
        }
        let right_v = mountainArr.get(right);
        if (right_v === target) {
            return right;
        }
        return -1
    }
    let mid = left + ((right - left) >> 1);
    let mid_v = mountainArr.get(mid);
    if (mid_v < target) {
        return findLeftTarget(mid+1, right, mountainArr, target)
    } else if (mid_v > target) {
        return findLeftTarget(left, mid, mountainArr, target)
    } else {
        return mid;
    }
    return -1
}
var findRightTarget = function(left, right, mountainArr, target) {
    if (right - left <= 1) {
        let left_v = mountainArr.get(left);
        if (left_v === target) {
            return left;
        }
        let right_v = mountainArr.get(right);
        if (right_v === target) {
            return right;
        }
        return -1
    }
    let mid = left + ((right - left) >> 1);
    let mid_v = mountainArr.get(mid);
    if (mid_v > target) {
        return findLeftTarget(mid+1, right, mountainArr, target)
    } else if (mid_v < target) {
        return findLeftTarget(left, mid, mountainArr, target)
    } else {
        return mid;
    }
    return -1
}
```

### <span id="kz">扩展 二分法详解</span>
##### 解释：
> 算法：当数据量很大适宜采用该方法。采用二分法查找时，数据需是**排好序**的。
 
 -  基本思想：假设数据是按升序排序的，对于给定值key，从序列的中间位置k开始比较， 如果当前位置arr[k]值等于key，则查找成功；
  - 若key小于当前位置值arr[k]，则在数列的前半段中查找,arr[low,mid-1]；
  - 若key大于当前位置值arr[k]，则在数列的后半段中继续查找arr[mid+1,high]，
  - 直到找到为止,时间复杂度:O(log(n))

##### 使用方法
 - 序列必须是有序的，无序序列无法使用二分法。
 - 通过递归查找，直至序列长度缩小到2或者1。

##### 模拟实现
> 以 nums[1,2,3,4,5]为例，找到数组中值为1的下标
1. left为 0，right为 4, 声明函数`find(left,right)`
2. 求出中间点 ` mid =  left + ((right - left) >> 1) ` 。 （>> 为位运算，相当于缩小2倍）
3. 得到 mid 为 2；判断 `nums[2] === 1 ? `，若等返回`mid`，nums[2] 为 3，不等 1 ，进入下一步
4. 判断`nums[mid] > 1`,由于nums[2] ===3 > 1,进入左序列`find(0, 2)`
5. 求出中间点 ` mid =  0+ ((2- 0) >> 1) `,
6. 得到mid 为 1；判断 `nums[1] === 1 ? `，若等返回`mid`，nums[1] 为 2，不等 1 ，进入下一步 
7. 判断`nums[mid] > 1`,由于nums[1] ===2 > 1,进入左序列`find(0, 1)`
8. 求出中间点 ` mid =  0+ ((1 - 0) >> 1) `,
9. 得到mid 为 0；判断 `nums[0] === 1 ? `，若等返回`mid`，nums[0] 为 1，等 1 ，return mid；

至此得到最后结果 下标为 0；
##### 代码实现

```javascript
let nums = [1,2,3,4,5] ;
let target = 1;
var find = function(left, right, target, nums) {
    let mid = (left + ((right - left) >> 1));  
    if (nums[mid] == target) {
        return mid;
    }
    if (nums[mid] > target) {
	    return find(left, mid, target, nums);
    }
    if (nums[mid] < target) {
        return find(mid + 1, right, target, nums);
    }
}
find(0,4,1, nums)
```
这种是数组中一定包含target的情况下。如果不确定是否包含，需要在值只剩下1-2个的时候做出判断。

```javascript
let nums = [1,2,3,4,5] ;
let target = 1;
var find = function(left, right, target, nums) {
	if (right- left <= 1) {
		return nums[left] === target ? left : (nums[right] ===target ? right: -1)
	}
    let mid = (left + ((right - left) >> 1));  
    if (nums[mid] == target) {
        return mid;
    }
    if (nums[mid] > target) {
	    return find(left, mid, target, nums);
    }
    if (nums[mid] < target) {
        return find(mid + 1, right, target, nums);
    }
}
find(0,4,1, nums)
```

这样如果不存在返回 -1 