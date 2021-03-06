---
title: 前端面试题
keywords: 前端, 面试, 面试题
description: 前端面试题, 包含一些手写题. 在脱离了电脑, 没有api提示, 进件白纸黑字处理, 会有极大的违和感.
date: 2019-11-06 18:00:00
categories: interview
tags: interview question
---

## 序言
2019年9月中旬，决定从之前的公司离职。恰逢家里有些事情，我在十月上旬的末尾重新回到上海，并与中旬开始投递简历。感觉不同于以前，不需要怎么复习就能拿 Offer，我开始去面试的时候，大多时候面壁。在这期间，想想还是写点 blog, 记录下来这次特殊的求职经历，并以此共勉。

### 常见函数
#### 斐波那函数
因兔子繁殖引入而又被称为"兔子数列". 特征是从第三项开始, 每一项都是前两项之和。

```javascript
// 用递归法实现斐波那契数列代码实现比较简洁，但在n比较大时会引起栈溢出，无法实现所需功能。
function fib(n) {
  if (n === 0) {
    return 0
  } else if (n === 1) {
    return 1
  } else {
    return fib(n - 1) + fib(n - 2)
  }
}

// 尾调用
function fib(n, ac1 = 1, ac2 = 1) {
  if (n <= 2) {
    return ac2
  }
  return fib(n - 1, ac2, ac1 + ac2)
}

```

### 知名公司及笔试题分析
笔试题其实对面试者来说，并不友好。实际开发时，开发者一般都有熟悉的ide，代码补全等工具进行辅助。当转移到了纸上，应聘者不仅需要牢记这些 api，留空白行的手感也完全不同。没有好好准备的话，不经浪费了大量的时间，而且还很容易被刷掉。因此，有换工作的同学，请务必提前3个月刷下题目，并练习下手写代码。

1. 百度众测
百度旗下的平台。稍微有点坑，对算法要求比较高。吐槽下，在JD里添加下重点考察算法如何。

1.1 实现排序
```javascript
// 在只检测是否为数组时, Array.isArray是一个更好的选项
function isArray(arr) {
  // return Object.prototype.toString.call(arr) === '[object Array]'
  return Array.isArray(arr)
}

function sort(arr) {
  if (!isArray(arr)){
    return [];
  }
  if (arr.length < 2) {
    return [];
  }
  var length = arr.length;
  var temp = [arr[0]];
  for (var i = 1; i < length; i++) {
    var tlen = temp.length;
    for(var j = 0; j < tlen; j++) {
      if(arr[i] <= temp[0]) {
        temp.unshift(arr[i]);
        break;
      }
      if (arr[i] >= temp[tlen - 1]) {
        temp.push(arr[i]);
        break;
      }
      if (arr[i] >= temp[j] && arr[i] <= temp[j+1]){
        temp.splice(j+1, 0, arr[i]);
        break;
      }
    }
  }
  return temp;
}
var arr = [5,4,3,2,1,2,3,6];
sort(arr); // (8) [1, 2, 2, 3, 3, 4, 5, 6]
```

1.2 查找长字符中重复字符最多的字符
```javascript
function filter(string){
  var acc = [];
  var temp = string.split('').sort();
  var length = string.length;
  var bcc = [];
  for (var i = 0; i < length; i++) {
    if (temp[i] !== bcc[0]) {
      acc.push(bcc);
      bcc = [];
      bcc.push(temp[i]);
    } else {
      bcc.push(temp[i]);
    }
    continue;
  }
  // return acc;
  console.log(acc); // [Array(0), Array(1), Array(5), Array(2), Array(6), Array(2)]
  var accLen = acc.length;
  var maxLen = 0;
  var maxInd = 0;
  for (var j = 0; j < accLen; j++) {
    if (acc[j].length > maxLen) {
      maxLen = acc[j].length;
      maxInd = j;
    }
  }
  return acc[maxInd];
}
var str = 'asjfsjfjsifjilsjflsjf';
filter(str); // ["j", "j", "j", "j", "j", "j"]
```


