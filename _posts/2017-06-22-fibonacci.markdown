---
layout: post
title: '菲波那切数列求和的JS方案及优化'
subtitle: ''
date: 2017-06-21 14:42:00
author: 'marillo'
header-img: 'img/about-bg.jpg'
catalog: true
tags:
    - JavaScript
---

使用递归的方式进行斐波那契数列求和如下：

```javascript
function fibonacci(n) {
  if (n === 0 || n === 1) {
    return n;
  } else {
    return fibonacci(n - 1) + fibonacci(n - 2);
  }
}
```

这种方式效率非常低，很多值会被重复求值。

下面我们用memoization方案进行优化

---



## memoization

memoization方案在《JavaScript模式》和《JavaScript设计模式》都有提到。它是一种将函数执行结果用变量缓存起来的方法。当函数进行计算之前，先看缓存对象中是否有次计算结果，如果有，就直接从缓存对象中获取结果；如果没有，就进行计算，并将结果保存到缓存对象中。

```javascript
let fibonacci = (function() {
  let memory = [];
  return function (n) {
    if (memory[n] !== undefined) {
      return memory[n];
    }
    return memory[n] = (n === 0 || n === 1) ? n : fibonacci(n-1) + fibonacci(n-2);
  }
})()
```

使用必报实现的memoization函数。测试通过之后，突然我有一个小疑问，如果将memory的类型由数组换成对象，它的运算效率会有什么变化？于是，我将memory的类型换成了对象，并写了一个函数测试两种数据类型的运算效率。

```javascript
function speed(n) {
  let start = performance.now();
  fibonacci(n);
  let end = performance.now();
  console.log(end-start);
}
```

> 所有测试只在Chrome控制台测试,并且测试次数不多，结果不严谨，请多多包涵。

memory类型为数组时（单位：毫秒）：

```javascript
speed(500)      // 0.8150000050663948
speed(5000)     // 3.1799999997019768
speed(7500)     // 4.234999991953373
speed(10000)    // 8.390000000596046
```

memory类型为对象时（单位：毫秒）：

```javascript
speed(500)      // 0.32499999552965164
speed(5000)     // 1.6499999985098839
speed(7500)     // 2.485000006854534
speed(10000)    // 2.9999999925494194
```

可以看出，memory类型为对象时明显比类型为数组时，运算速度快很多，这是因为，我们调用fibonacci(100)，这时候，**fibonacci函数在第一次计算的时候会设置memory[100]=xxx，此时数组的长度是101，而前面100项会被初始化为undefined。**正因为如此，memory的类型为数组的时候比类型是对象的时候慢。

---

## Best Solution

别人的解决方案给了我灵感，让我想出了一个缓存效率高很多的方案。

```javascript
var fibonacci = (function () {
  var memory = {}
  return function(n) {
    if(n==0 || n == 1) {
      return n
    }
    if(memory[n-2] === undefined) {
      memory[n-2] = fibonacci(n-2)
    }
    if(memory[n-1] === undefined) {
      memory[n-1] = fibonacci(n-1)
    }
    return memory[n] = memory[n-1] + memory[n-2]
  }
})()
```

测试结果就不放了（因为我发现在Chrome控制台中运行测试代码时，输出结果不稳定）。不过，这里的缓存效率的确是提高了，前面的方案，一次计算最多缓存一个结果，而这个方案，一次计算最多缓存三个结果。从这个方面考虑，运算速度理论上是会比前面的方案快的。

## 动态规划解决方案

斐波那契数列求和除了可以用递归的方法解决，还可以用动态规划的方法解决。由于我是算法渣，对动态规划了解不多，只懂一点点皮毛，所以这里就不解释动态规划的概念了。

直接贴代码好了：

```javascript
function fibonacci(n) {
    let n1 = 1,
        n2 = 1,
        sum = 1
    for(let i = 3; i <= n; i += 1) {
        sum = n1 + n2
        n1 = n2
        n2 = sum
    }
    return sum
}
```

## 尾递归方案

在ES6规范中，有一个[尾调用优化](http://es6.ruanyifeng.com/#docs/function#%E5%B0%BE%E8%B0%83%E7%94%A8%E4%BC%98%E5%8C%96)，可以实现高效的尾递归方案。

```javascript
'use strict'
function fibonacci(n, n1, n2) {
    if(n <= 1) {
        return n2
    }
    return fibonacci(n - 1, n2, n1 + n2)
}
```

> ES6的尾调用优化只在严格模式下开启，正常模式是无效的。

## 通项公式方案

斐波那契数列是有[通项公式](http://baike.baidu.com/view/816.htm#2_2)的，但通项公式中有开方运算，在js中会存在误差，而`fib函数`中的Math.round正式解决这一问题的。

```javascript
function fibonacci(n){
    var sum = 0
    for(let i = 1; i <= n; i += 1) {
        sum += fib(i)
    }
    return sum

    function fib(n) {
      const SQRT_FIVE = Math.sqrt(5);
      return Math.round(1/SQRT_FIVE * (Math.pow(0.5 + SQRT_FIVE/2, n) - Math.pow(0.5 - SQRT_FIVE/2, n)));
    }
}
```



