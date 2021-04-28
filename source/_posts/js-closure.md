---
layout: '[layout]'
title: JS基础解析-闭包
keywords: 'keywords'
description: '附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面'
date: '2019-12-15 18:00:00'
tags: ['JS基础', '闭包']
---

## 前言
JS基础解析系列, 主要是为了弄清楚JS中一些常见但容易出错，不常见但有助于理解代码的要点。不定时更新，请多多见谅。
闭包是JS中很常见的一种写法。本质是利用函数的嵌套，和函数的作用域(内部函数访问外部变量)，来创建出模拟的块级作用域。利用闭包，基本的目的是创造一个封闭的变量，避免污染全局变量，来完成包的封装。

## 使用示例
闭包常见的使用模式如下:
```javascript
/**
 * 防抖函数
 * @param {function} fn, 待执行的函数
 * @param {number} wait, 延时的时间(单位毫秒)
 */
function debounce(fn, wait = 300) {
  let timer
  return function() {
    const args = arguments
    const context = this
    if (timer) {
      clearTimeout(timer)
    }
    timer = setTimeout(() => {
      fn.apply(context, args)
    }, wait)
  }
}

```
以上是简版的防抖函数. 不过也将闭包的一些点展示了出来：
1. 内部函数访问外部函数的变量
2. 内部函数this的指向问题
3. 内存泄漏问题

### 第一个问题，也是闭包必要性的根本。
假如不使用闭包，要如何实现上面的功能。简洁一点的实现是直接定义一个或者多个全局变量。但定义了全局变量，更多的问题也随之而来。比较棘手的问题有：全局变量污染，内存占用。这些都可能会导致严重的不可预知的bug。使用闭包，最根本上就是为了规避这种难题。通过模拟一个块级作用域，将变量限定在一个范围内，减少对其它代码的影响。

### 第二个问题，保证this指向一致性。
this的指向也一直是个难题。假如不做处理，this会默认指向当前执行的上下文(context)。以上面的防抖函数为例，为了保持this指向的一致性，就需要一些额外的处理。通过apply传递this, 以及函数的默认参数(arguments), 将当前环境，以及参数传递给需要的执行函数fn，以此来保证this和参数的一致性。在常见的dom操作中，这样就保证了dom和mouseEvent的传递。

### 第三个问题，内存泄漏
由于闭包导致的内存泄漏，实质上是IE9之前的版本对JScript对象和COM对象使用不同的垃圾回收机制。
```javascript
// 常见的遍历dom
function foo () {
  var element = document.getElementById('someElement')

  // 只要匿名函数存在，element引用数至少为1
  element.onclick = function () {
    alert(element.id) // 闭包内，一直执行的是外层引用的element，闭包执行完后，element的引用数无法减少，导致无法释放销毁
  }
}

```
除此之外，闭包本身不会因此而造成内存泄漏.

## 总结
闭包本身的概念是相对简单的，但也是应用极为广泛的存在. 虽然实际应用中会遇到各种变种，但详细掌握后，基本上就不会存在理解的障碍。

## 引用
1. [聊聊闭包那些事](https://juejin.cn/post/6955309404207972360/)
