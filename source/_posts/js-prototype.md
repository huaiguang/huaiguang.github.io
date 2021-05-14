---
title: JS基础-原型
keywords: keywords
description: 原型是JS中很重要的一个概念，JS中面向对象编程的基础。所有的对象都有其原型，以及对象上`__proto__`属性链接起来的原型链，是JS这门语言的构成的基础。理解好原型，在阅读源码，编写组件等诸多方面，有相当的益处。
categories: JS base
tags: js prototype
date: 2019-12-08 18:00:00
---

## 前言
JS基础解析系列, 主要是为了弄清楚JS中一些常见但容易出错，不常见但有助于理解代码的要点。不定时更新，请多多见谅。

原型是JS中很重要的一个概念，JS中面向对象编程的基础。所有的对象都有其原型, 所有对象上`__proto__`属性链接起来的原型链，是JS这门语言的构成的基础。理解好原型，在阅读源码，编写组件等诸多方面，有相当的益处。

## 原型(prototype)
JS 中每个函数都有一个`prototype`原型对象, 例如`Object.prototype`. 比较常见的代码如下:
```javascript
function Point(x, y) {
  this.x = x
  this.y = y
}

Point.prototype.say = function() {
  return `x is ${this.x}, y is ${this.y}`
}

const pointA = new Point(1,2)
console.log(pointA) // Point { x: 1, y: 2 }
const sayA = pointA.say()
console.log('say:', sayA) // say: x is 1, y is 2

```
如果仔细一点的话，可以在以上的代码中发现一些特别的地方：
- 定义的`Point`函数首字母大写
- 在`console`中打印的`Point`对象中，除去定义的属性x, y, 还有一个`__proto__`属性. 在`__proto__`属性中, 有上面显示定义的`say`函数, 还有`constructor`(*指向Point函数*), 以及`__proto__`(*指向Object.prototype*).

### 构造函数(`constructor`)
类似于上面定义的 Point函数, 这类函数叫做构造函数。比较常见的构造函数为Object, Array, String等等。通过new关键字, 构造函数可以创建一个对象。

new关键字创建对象的过程如下:
1. 创建一个继承于构造函数的prototype属性的对象
2. 调用构造函数, 并将步骤1新创建的对象作为this的上下文
3. 如果构造函数的是一个对象, 则返回这个对象；否则返回的是上述步骤中新创建的this.

返回了对象，覆盖了定义的对象类型的情况暂不讨论。这里集中于没有返回对象，按照定义的对象类型创建了对象示例的情况. 经构造函数创建的所有实例，共享构造函数的prototype，这也是js继承的基础。

### `prototype和__proto__属性`
*每一个方法都有原型prototype，每一个对象都有一个__proto__.* 每个对象的__proto__属性指向其构造函数的 prototype 属性。构造函数本身作为对象，其__proto__属性指向`Function.prototype`
来做一些有趣的实验, 如下的表达式是true or false.
```javascript
pointA.__proto__ === Point.prototype
pointA.__proto__.constructor === Point
Point.__proto__ === Function.prototype
Point.prototype.__proto__ === Object.prototype
Point.prototype.constructor === Point
Function.prototype.__proto__ === Object.prototype
Object.prototype.__proto__ === null

```
稍微花费点时间考虑下，或许收获会更多。

### 原型链(`__proto__`)
所有创建的实例，都可以通过__proto__属性连接其构造函数的prototype. 例如上文中pointA.x和pointA.say都可以访问, pointA.x是显式访问，pointA.say则是隐式访问.
隐式访问是通过原型链来实现的。实例对象与原型之间的连接，叫做原型链。当在对象中查找某个属性时，如果在当前的对象中能够查到，该直接返回该属性；如果在当前的对象中查找不到，则会顺着原型链，即__proto__属性指向的对象往上查找，直到到达链路的源头。

### 继承
```javascript
function Point(x, y) {
  this.x = x
  this.y = y
}

Point.prototype.say = function() {
  return `x is ${this.x}, y is ${this.y}`
}

const pointA = new Point(1,2)
const pointB = new Point(3,4)

// JS中两个对象不会全等, 只有相同的对象引用才会全等
pointA.x === 1 // true
pointB.x === 3 // true
pointA.say === pointB.say // true
```

落实到代码中, 也就是创建类似class的继承。通过构造函数创建的对象，定义的属性是各自的，但__proto__指向的constructor的prototype是共享的。在ES2015中，可以直接通过关键戏class来创建对象了，不过polyfill的实现也是基于此。
上述是基本的继承，在实际应用中，会为了满足需求，做一些封装，不过那是另一个篇章了。

## 引用
1. [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)
