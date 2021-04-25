---
layout: '[layout]'
title: JS要点解析-this
date: 2019-12-01 18:01:40
tags: '前端, 面试, 手写代码'
---

## 前言
JS要点解析系列, 主要是为了弄清楚JS中一些常见但容易出错，不常见但有助于理解代码的要点。不定时更新，请多多见谅。

## this
this是JS中绕不过的要点，方方面面都和其相关。在技术的变迁中，this的疑问也有各种各种的变化。下面就将按照不同的背景，讲述下this的指向问题。

## 在ES2015之前
在ES6之前, 最常用的包还是jQuery的时代. 主流的方式是通过构造函数，模拟面向对象来编写代码.
常见用法如下:

```javascript
// 不指定
// 严格模式下, this为undefined,
// 'use strict'
// 非严格模式, 下为全局变量window
function show() {
  this.a = 'Hello world!'
}
show()

// 直接在元素上添加方法
document.getElementById('test').onclick = function() {
  // this指向id为test的元素
  this.innerHTML = 'Hello world!'
}

// 构造函数+new
function Point() {
  // this指向当前创建的对象
  this.x = x
  this.y = y
}
```

## 在ES2015之后
ES6中引入了箭头函数(=>), 增加了一种this的使用方法. 配合上Vue, React等框架，降低了前端开发的难度，使得前端可以更加的专注于业务.
常见的用法如下:

```javascript
// 箭头函数
// 常见的Vue中reload方法
this.$nextTick(() => {
  this.reload()
})
```

## 特殊调用
在JS中, 为了提高代码的效率, 提供了几个可以改变this指向的函数call, apply和bind. 虽然和本文介绍this指向的关系不大, 但稍微提及下.
常见的调用方式如下:
```javascript
// call & apply
// attention: call的参数从第二个起是单独的参数，apply的第二个参数是数组
// 将node节点列表的类数组转化为真正的数组
Array.prototype.slice.call(nodeList)

// bind
// attention: bind函数返回的是一个方法
// studentA的对象绑定了sayName方法
Person.sayName.bind(studentA)
```

## 总结
**this在JS中指向当前的上下文环境(context), 指向取决于包含它的函数的调用位置.**
实际使用中, 如何确定当前的上下文环境并不是一件容易的事情. 除了常见的点操作符(.)和中括号操作符([])等在代码中显示的绑定指向外，比较麻烦的是在方法中(function)声明的this, 在执行时才能确定this指向的隐式绑定.
