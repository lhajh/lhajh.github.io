---
layout: post
title: 正则实现数学运算
categories: [js]
description: 正则实现数学运算
keywords: js, reg
---

带你认识一下正则的强大！！！

## 和

```js
function sum(n, m) {
  n = Array(n + 1).join('#')
  m = Array(m + 1).join('#')
  return n.replace(/$/, m).length
}
sum(3, 2)
// => 5
```

## 差

```js
function diff(n, m) {
  n = Array(n + 1).join('#')
  m = Array(m + 1).join('#')
  return n.replace(m, '').length
}
diff(3, 2)
// => 1
```

## 积

```js
function product(n, m) {
  n = Array(n + 1).join('#')
  m = Array(m + 1).join('#')
  return n.replace(/./g, m).length
}
product(3, 2)
// => 6
```

## 商

```js
function division(n, m) {
  n = Array(n + 1).join('#')
  m = Array(m + 1).join('#')
  return n.replace(new RegExp(m, 'g'), '#').length
}
division(6, 2)
// => 3
```

## 余

```js
function remainder(n, m) {
  n = Array(n + 1).join('#')
  m = Array(m + 1).join('#')
  return n.replace(new RegExp(m, 'g'), '').length
}
remainder(7, 2)
// => 1
```

## 平方

```js
function square(n) {
  n = Array(n + 1).join('#')
  return n.replace(/./g, n).length
}
square(7)
// => 49
```

## 奇偶性

```js
function isOdd(n) {
  n = Array(n + 1).join('#')
  return /^.(.{2})+$/.test(n)
}
isOdd(7)
// => true
```

## 素数

```js
function isPrime(n) {
  n = Array(n + 1).join('#')
  return /^(?!(.{2,})\1+$)(?=.{2,}$)/.test(n)
}
isPrime(11)
// => true
```

## 最大公约数

```js
function greatestCommonDivisor(n, m) {
  n = Array(n + 1).join('#')
  m = Array(m + 1).join('#')
  return `${n}-${m}`.match(/^(.+)\1*-\1+$/)[1].length
}
greatestCommonDivisor(12, 8)
// => 4
```

## 参考资料

- [正则实现数学运算 - 掘金](https://juejin.im/post/5c348695e51d4551ec60850e)
