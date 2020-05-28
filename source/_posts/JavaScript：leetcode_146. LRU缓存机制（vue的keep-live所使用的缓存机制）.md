---
title: JavaScript：leetcode_146. LRU缓存机制（vue的keep-live所使用的缓存机制）
date: 2020-05-26 17:59:13
categories: ['Algorithm','每日一题']
tags: Algorithm
keywords: leetcode, LRU缓存机制
---

### 题目说明
```
运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

 
进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作？
```

### 解题思路一
1. [LRU缓存机制](https://baike.baidu.com/item/LRU)，可以自行百度一下。
	1. 特点1，hash表读取数据
	2. 特点2，存在一个`keys`序列，代表缓存的所有`key`，顺序按照最近的`活跃度`来排序，比如你`刚刚`用过`key为1` 的值，那么`1`就会排在`keys序列`的第`一`位。当缓存`超出`的时候，会优先`删除keys`的`末尾`。
2. 所以我们主要维护了一个hash，js中就是一个对象，用来存数据。一个序列也就是一个数组存keys。
3. get：如果将get的key，位置置换到首位。并返回数据。
4. put：将put设置的值的key，放在keys序列首位，判断是否超出，超出则删除最后一位。
### 代码实现一
```javascript
/**
 * @param {number} capacity
 */
var LRUCache = function(capacity) {
    this.obj = {};
    this.objKeys = [];
    this.limit = capacity;
};

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function(key) {
    if (this.obj[key]) {
        this.objKeys.splice(this.objKeys.indexOf(key), 1);
        this.objKeys.unshift(key);
        return this.obj[key];
    } else {
        return -1
    }
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function(key, value) {
    this.obj[key] && this.objKeys.splice(this.objKeys.indexOf(key), 1);
    this.objKeys.unshift(key);
    this.obj[key] = value;
    if (this.objKeys.length > this.limit) {
        delete this.obj[this.objKeys[this.limit]];
        this.objKeys.length -= 1;
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```