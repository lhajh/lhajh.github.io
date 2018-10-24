---
layout: post
title: JS 斐波那契数列的三种算法实现
categories: JS
description: JS 实现斐波那契数列一般会用到迭代、递归和尾递归，但是 JS 并没有对尾递归进行优化支持。
keywords: js, 算法
---

## 斐波那契数列

形如 1,1,2,3,5... 的数列就是斐波那契数列。其通项 fibonacci(n) = fibonacci(n-1) + fibonacci(n-2)。

## 递归算法

```js
var numbers = []
function fibonacciRecursion(n) {
  if (numbers[n] !== undefined) {
    return numbers[n]
  }
  if (n === 1 || n === 2) {
    numbers[n] = 1
  } else {
    numbers[n] = fibonacciRecursion(n - 1) + fibonacciRecursion(n - 2)
  }
  return numbers[n]
}
```

思路：使用 numbers[n] 记录序号为 n 的数列项值，避免多次运算。递归调用函数实现通项公式。

问题：n 值过大时会出现函数调用栈溢出的情况。

## 迭代算法

```js
function fibonacciLoop(n) {
  var result = 0,
    former = 1,
    temp = 0
  for (var i = 1; i <= n; i++) {
    if (i <= 2) {
      result = 1
    } else {
      temp = result
      result += former
      former = temp
    }
  }
  return result
}
```

思路：定义 3 个变量，result 保存 febonacci(n-1)，former 保存 febonacci(n-2),temp 作为临时变量保存交换前的数据。在下一次循环中将两个值相加得 febonacci(n)。

问题：代码量大，不够简洁。

## 尾递归

尾递归指函数最后的返回值只有一个函数，不包括任何运算符。

```js
function fibonacciTail(n, former, result) {
  'use strict'
  if (n === 1 || n === 2) {
    return result
  }
  return fibonacciTail(n - 1, result, former + result)
}
```

思路： 事实上思路与迭代一致，将迭代中的变量转变为形参，这样还省去了 temp 变量。同时不需要担心递归调用导致的栈溢出，因为函数每次执行到最后相当于将当前函数出栈，新函数入栈。

问题： 仅 ES6 的严格模式下支持尾递归优化。
